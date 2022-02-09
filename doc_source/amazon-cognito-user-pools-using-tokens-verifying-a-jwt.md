# Verifying a JSON web token<a name="amazon-cognito-user-pools-using-tokens-verifying-a-jwt"></a>

These steps describe verifying a user pool JSON Web Token \(JWT\)\.

**Topics**
+ [Prerequisites](#amazon-cognito-user-pools-using-tokens-prerequisites)
+ [Step 1: Confirm the structure of the JWT](#amazon-cognito-user-pools-using-tokens-step-1)
+ [Step 2: Validate the JWT signature](#amazon-cognito-user-pools-using-tokens-step-2)
+ [Step 3: Verify the claims](#amazon-cognito-user-pools-using-tokens-step-3)

## Prerequisites<a name="amazon-cognito-user-pools-using-tokens-prerequisites"></a>

The tasks in this section might be already handled by your library, SDK, or software framework\. For example, user pool token handling and management are provided on the client side through the Amazon Cognito SDKs\. Likewise, the Mobile SDK for iOS and the Mobile SDK for Android automatically refresh your ID and access tokens if two conditions are met: A valid \(unexpired\) refresh token must present, and the ID and access tokens must have a minimum remaining validity of 5 minutes\. For information on the SDKs, and sample code for JavaScript, Android, and iOS see [Amazon Cognito user pool SDKs](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

Many good libraries are available for decoding and verifying a JSON Web Token \(JWT\)\. Such libraries can help if you need to manually process tokens for server\-side API processing or if you are using other programming languages\. See the [OpenID foundation list of libraries for working with JWT tokens](http://openid.net/developers/jwt/)\.

## Step 1: Confirm the structure of the JWT<a name="amazon-cognito-user-pools-using-tokens-step-1"></a>

A JSON Web Token \(JWT\) includes three sections:

1. Header

1. Payload

1. Signature


|  |
| --- |
|  11111111111\.22222222222\.33333333333  |

These sections are encoded as base64url strings and are separated by dot \(\.\) characters\. If your JWT does not conform to this structure, consider it invalid and do not accept it\.

## Step 2: Validate the JWT signature<a name="amazon-cognito-user-pools-using-tokens-step-2"></a>

The JWT signature is a hashed combination of the header and the payload\. Amazon Cognito generates two pairs of RSA cryptographic keys for each user pool\. One of the private keys is used to sign the token\.

**To verify the signature of a JWT token**

1. Decode the ID token\.

   You can use AWS Lambda to decode user pool JWTs\. For more information see [Decode and verify Amazon Cognito JWT tokens using Lambda](https://github.com/awslabs/aws-support-tools/tree/master/Cognito/decode-verify-jwt)\.

   The OpenID Foundation also [maintains a list of libraries for working with JWT tokens](http://openid.net/developers/jwt/)\.

1. Compare the local key ID \(`kid`\) to the public kid\.

   1. Download and store the corresponding public JSON Web Key \(JWK\) for your user pool\. It is available as part of a JSON Web Key Set \(JWKS\)\. You can locate it at https://cognito\-idp\.\{region\}\.amazonaws\.com/\{userPoolId\}/\.well\-known/jwks\.json\.

      For more information on JWK and JWK sets, see [JSON web key \(JWK\)](https://tools.ietf.org/html/rfc7517)\.
**Note**
This is a one\-time step before your web API operations can process tokens\. Now you can perform the following steps each time the ID token or the access token are used with your web API operations\.

      This is a sample `jwks.json` file:

      ```
      {
      	"keys": [{
      		"kid": "1234example=",
      		"alg": "RS256",
      		"kty": "RSA",
      		"e": "AQAB",
      		"n": "1234567890",
      		"use": "sig"
      	}, {
      		"kid": "5678example=",
      		"alg": "RS256",
      		"kty": "RSA",
      		"e": "AQAB",
      		"n": "987654321",
      		"use": "sig"
      	}]
      }
      ```
**Note**
The two keys returned in the `jwks.json` file will verify different JWTs returned from a call to Cognito's [`InitiateAuth`](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html#API_InitiateAuth_ResponseSyntax) command. The first key (`keys[0]`) will validate the `AuthenticationResult.IdToken`. The second key (`keys[1]`) will validate the `AuthenticationResult.AccessToken`.

**Key ID \(`kid`\)**
The `kid` is a hint that indicates which key was used to secure the JSON web signature \(JWS\) of the token\.
**Algorithm \(`alg`\)**
The `alg` header parameter represents the cryptographic algorithm that is used to secure the ID token\. User pools use an RS256 cryptographic algorithm, which is an RSA signature with SHA\-256\. For more information on RSA, see [RSA cryptography](https://tools.ietf.org/html/rfc3447)\.
**Key type \(`kty`\)**
The `kty` parameter identifies the cryptographic algorithm family that is used with the key, such as "RSA" in this example\.
**RSA exponent \(`e`\)**
The `e` parameter contains the exponent value for the RSA public key\. It is represented as a Base64urlUInt\-encoded value\.
**RSA modulus \(`n`\)**
The `n` parameter contains the modulus value for the RSA public key\. It is represented as a Base64urlUInt\-encoded value\.
**Use \(`use`\)**
The `use` parameter describes the intended use of the public key\. For this example, the `use` value `sig` represents signature\.

   1. Search the public JSON web key for a `kid` that matches the `kid` of your JWT\.

1. Use a JWT library, such as the [aws\-jwt\-verify library on GitHub](https://github.com/awslabs/aws-jwt-verify), to compare the signature of the issuer to the signature in the token\. The issuer signature is derived from the public key \(the RSA modulus `"n"`\) of the `kid` in jwks\.json that matches the token `kid`\. You might need to convert the JWK to PEM format first\. The following example takes the JWT and JWK and uses the Node\.js library, jsonwebtoken, to verify the JWT signature:

------
#### [ Node\.js ]

   ```
   var jwt = require('jsonwebtoken');
   var jwkToPem = require('jwk-to-pem');
   var pem = jwkToPem(jwk);
   jwt.verify(token, pem, { algorithms: ['RS256'] }, function(err, decodedToken) {
   });
   ```

------

## Step 3: Verify the claims<a name="amazon-cognito-user-pools-using-tokens-step-3"></a>

**To verify JWT claims**

1. Verify that the token is not expired\.

1. The audience \(`aud`\) claim should match the app client ID that was created in the Amazon Cognito user pool\.

1. The issuer \(`iss`\) claim should match your user pool\. For example, a user pool created in the `us-east-1` Region will have the following `iss` value:

   `https://cognito-idp.us-east-1.amazonaws.com/<userpoolID>`\.

1. Check the `token_use` claim\.
   + If you are only accepting the access token in your web API operations, its value must be `access`\.
   + If you are only using the ID token, its value must be `id`\.
   + If you are using both ID and access tokens, the `token_use` claim must be either `id` or `access`\.

You can now trust the claims inside the token\.
