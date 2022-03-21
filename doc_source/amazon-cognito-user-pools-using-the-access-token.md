# Using the access token<a name="amazon-cognito-user-pools-using-the-access-token"></a>

The user pool access token contains claims about the authenticated user, a list of the user's groups, and a list of scopes\. The purpose of the access token is to authorize API operations in the context of the user in the user pool\. For example, you can use the access token to grant your user access to add, change, or delete user attributes\.

The access token is represented as a JSON Web Token \(JWT\)\. The header for the access token has the same structure as the ID token\. However, the key ID \(`kid`\) is different because different keys are used to sign ID tokens and access tokens\. As with the ID token, you must first verify the signature of the access token in your web APIs before you can trust any of its claims\. See [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\. You can set the access token expiration to any value between 5 minutes and 1 day\. You can set this value per app client\.

**Important**  
For access and ID tokens, don't specify a minimum less than an hour\. Amazon Cognito HostedUI uses cookies that are valid for an hour\. If you enter a minimum less than an hour, you won't get a lower expiry time\.

## Access token header<a name="user-pool-access-token-header"></a>

The header contains two pieces of information: the key ID \(`kid`\), and the algorithm \(`alg`\)\.

```
{
"kid" : "1234example="
"alg" : "RS256",
}
```

**Key ID \(`kid`\)**  
The `kid` parameter is a hint indicating which key was used to secure the JSON Web Signature \(JWS\) of the token\.  
For more information about the `kid` parameter, see the [Key identifier \(kid\) header parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.5)\.

**Algorithm \(`alg`\)**  
The `alg` parameter represents the cryptographic algorithm that was used to secure the access token\. User pools use an RS256 cryptographic algorithm, which is an RSA signature with SHA\-256\.  
For more information about the `alg` parameter, see [Algorithm \(alg\) header parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.4)\.

## Access token payload<a name="user-pool-access-token-payload"></a>

This is a sample payload from an access token\. For more information, see [JWT claims](https://tools.ietf.org/html/rfc7519#section-4)\.

```
{
  "sub": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  "device_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  "cognito:groups": [
    "admin"
  ],
  "token_use": "access",
  "scope": "aws.cognito.signin.user.admin",
  "auth_time": 1562190524,
  "iss": "https://cognito-idp.us-west-2.amazonaws.com/us-west-2_example",
  "exp": 1562194124,
  "iat": 1562190524,
  "origin_jti": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  "jti": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  "client_id": "57cbishk4j24pabc1234567890",
  "username": "janedoe@example.com"
}
```

**Subject \(`sub`\)**  
The `sub` claim is a unique identifier \(UUID\) for the authenticated user\. It is not the same as the user name, which may not be unique\.

**Amazon Cognito groups \(`cognito:groups`\)**  
The `cognito:groups` claim is a list of groups the user belongs to\.

**Token use \(`token_use`\)**  
The `token_use` claim describes the intended purpose of this token\. Its value is always `access` in the case of the access token\.

**Scope \(`scope`\)**  
The scope claim is a list of Oauth 2\.0 scopes that define what access the token provides\. 

**Authentication time \(`auth_time`\)**  
The `auth_time` claim contains the time when the authentication occurred\. Its value is a JSON number that represents the number of seconds from 1970\-01\-01T0:0:0Z as measured in UTC format\. On refreshes, it represents the time when the original authentication occurred, not the time when the token was issued\.

**Issuer \(`iss`\)**  
The `iss` claim has the following format:  
https://cognito\-idp\.\{region\}\.amazonaws\.com/\{userPoolId\}\.

**Origin jti \(`origin_jti`\)**  
The originating JWT identifier, from the time when the original authentication occurred\.

**jti \(`jti`\)**  
The `jti` claim is the unique identifier of the JWT\.

## Access token signature<a name="user-pool-access-token-signature"></a>

The signature of the access token is calculated based on the header and payload of the JWT token\. When used outside of an application in your web APIs, you must always verify this signature before accepting the token\. For more information, see [JWT tokens](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-verifying-a-jwt.html)\.