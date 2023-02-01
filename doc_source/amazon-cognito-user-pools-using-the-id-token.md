# Using the ID token<a name="amazon-cognito-user-pools-using-the-id-token"></a>

The ID token is a [JSON web token \(JWT\)](https://tools.ietf.org/html/rfc7519) that contains claims about the identity of the authenticated user, such as `name`, `email`, and `phone_number`\. You can use this identity information inside your application\. The ID token can also be used to authenticate users to your resource servers or server applications\. You can also use an ID token outside of the application with your web API operations\. In those cases, you must verify the signature of the ID token before you can trust any claims inside the ID token\. See [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\. 

You can set the ID token expiration to any value between 5 minutes and 1 day\. You can set this value per app client\.

**Important**  
When your user signs in with the hosted UI or a federated identity provider \(IdP\), Amazon Cognito sets session cookies that are valid for 1 hour\. If you use the hosted UI or federation, and specify a minimum duration of less than 1 hour for your access and ID tokens, your users will still have a valid session until the cookie expires\. If the user has tokens that expire during the one\-hour session, the user can refresh their tokens without the need to reauthenticate\.

**ID Token Header**  
The header contains two pieces of information: the key ID \(`kid`\), and the algorithm \(`alg`\)\.

```
{
"kid" : "1234example=",
"alg" : "RS256"
}
```

**Key ID \(`kid`\)**  
The `kid` parameter is a hint that indicates which key was used to secure the JSON Web Signature \(JWS\) of the token\.  
For more information about the `kid` parameter, see the [Key identifier \(kid\) header parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.5)\.

**Algorithm \(`alg`\)**  
The `alg` parameter represents the cryptographic algorithm that is used to secure the ID token\. User pools use an RS256 cryptographic algorithm, which is an RSA signature with SHA\-256\.  
For more information about the `alg` parameter, see [Algorithm \(alg\) header parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.4)\.

**ID Token Payload**  
This is a sample payload from an ID token\. It contains claims about the authenticated user\. For more information about OpenID Connect \(OIDC\) standard claims, see the [OIDC standard claims](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\.

```
      {
      "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "aud": "xxxxxxxxxxxxexample",
      "email_verified": true,
      "token_use": "id",
      "auth_time": 1500009400,
      "iss": "https://cognito-idp.us-east-1.amazonaws.com/us-east-1_example",
      "cognito:username": "janedoe",
      "exp": 1500013000,
      "given_name": "Jane",
      "iat": 1500009400,
      "email": "janedoe@example.com",
      "jti": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "origin_jti": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
      
      }
```

**Subject \(`sub`\)**  
The `sub` claim is a unique identifier \(UUID\) for the authenticated user\. It is not the same as the user name, which might not be unique\.

**Issuer \(`iss`\)**  
The `iss` claim has the following format:  

```
https://cognito-idp.{region}.amazonaws.com/{userPoolId}
```
For example, suppose you created a user pool in the `us-east-1` Region and its user pool ID is `u123456`\. In that case, the ID token that is issued for users of your user pool have the following `iss` claim value:  

```
https://cognito-idp.us-east-1.amazonaws.com/u123456
```

**Audience \(`aud`\)**  
The content of the `aud` claim is the `client_id` that the user requested when they authenticated with your user pool\.

**Token use \(`token_use`\)**  
`The token_use` claim describes the intended purpose of this token\. Its value is always `id` in the case of the ID token\.

**Authentication time \(`auth_time`\)**  
The `auth_time` claim contains the time when the authentication occurred\. Its value is a JSON number representing the number of seconds from 1970\-01\-01T0:0:0Z as measured in UTC format\. On refreshes, `auth_time` represents the time when the original authentication occurred, not the time when the token was issued\.

**Nonce \(`nonce`\)**  
The `nonce` claim comes from a parameter of the same name that you can add to requests to your OAuth 2\.0 `authorize` endpoint\. When you add the parameter, the `nonce` claim is included in the ID token that Amazon Cognito issues, and you can use it to guard against replay attacks\. If you do not provide a `nonce` value in your request, Amazon Cognito automatically generates and validates a nonce when you authenticate through a third\-party identity provider, then adds it as a `nonce` claim to the ID token\. The implementation of the `nonce` claim in Amazon Cognito is based on [OIDC standards](https://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation)\.

**Origin jti \(`origin_jti`\)**  
The originating JWT identifier, from the time when the original authentication occurred\.

**jti \(`jti`\)**  
The `jti` claim is a unique identifier of the JWT\.

The ID token can contain OIDC standard claims that are defined in [OIDC standard claims](http://openid.net/specs/openid-connect-core-1_0.html#Claims)\. The ID token can also contain custom attributes that you define in your user pool\. Amazon Cognito writes custom attribute values to the ID token as strings regardless of attribute type\.

**Note**  
User pool custom attributes are always prefixed with a custom: prefix\. 

**ID Token Signature**  
The signature of the ID token is calculated based on the header and payload of the JWT token\. When used outside of an application in your web APIs, you must always verify this signature before accepting the token\. See Verifying a JSON Web Token\. [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\.