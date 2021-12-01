# Using the ID token<a name="amazon-cognito-user-pools-using-the-id-token"></a>

The ID token is a [JSON web token \(JWT\)](https://tools.ietf.org/html/rfc7519) that contains claims about the identity of the authenticated user such as `name`, `email`, and `phone_number`\. You can use this identity information inside your application\. The ID token can also be used to authenticate users to your resource servers or server applications\. You can also use an ID token outside of the application with your web API operations\. In those cases, you must verify the signature of the ID token before you can trust any claims inside the ID token\. See [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\. 

You can set the ID token expiration to any value between 5 minutes and 1 day\. This value can be set per app client\.

**Important**  
For access and ID tokens, don't specify a minimum less than an hour\. Amazon Cognito HostedUI uses cookies that are valid for an hour, if you enter a minimum less than an hour, you won't get a lower expiry time\.

**ID Token Header**  
The header contains two pieces of information: the key ID \(`kid`\), and the algorithm \(`alg`\)\.

```
{
"kid" : "1234example="
"alg" : "RS256",
}
```

**Key ID \(`kid`\)**  
The `kid` parameter is a hint that indicates which key was used to secure the JSON Web Signature \(JWS\) of the token\.  
For more information about the kid parameter, see the [Key identifier \(kid\) header parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.5)\.

**Algorithm \(`alg`\)**  
The `alg` parameter represents the cryptographic algorithm that is used to secure the ID token\. User pools use an RS256 cryptographic algorithm, which is an RSA signature with SHA\-256\.  
For more information about the `alg` parameter, see [Algorithm \(alg\) header parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.4)\.

**ID Token Payload**  
This is a sample payload from an ID token\. It contains claims about the authenticated user\. For more information about OIDC standard claims, see the [OIDC standard claims](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\.

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
The `sub` claim is a unique identifier \(UUID\) for the authenticated user\. It is not the same as the user name which might not be unique\.

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
The `aud` claim contains the `client_id` that is used in the user authentication\. 

**Token use \(`token_use`\)**  
`The token_use` claim describes the intended purpose of this token\. Its value is always `id` in the case of the ID token\.

**Authentication time \(`auth_time`\)**  
The `auth_time` claim contains the time when the authentication occurred\. Its value is a JSON number representing the number of seconds from 1970\-01\-01T0:0:0Z as measured in UTC format\. On refreshes, it represents the time when the original authentication occurred, not the time when the token was issued\.

**Origin jti \(`origin_jti`\)**  
The originating JWT identifier, from when the original authentication occurred\.

**jti \(`jti`\)**  
The `jti` claim is a unique identifier of the JWT\.

The ID token can contain OpenID Connect \(OIDC\) standard claims that are defined in [OIDC standard claims](http://openid.net/specs/openid-connect-core-1_0.html#Claims)\. It can also contain custom attributes that you define in your user pool\.

**Note**  
User pool custom attributes are always prefixed with a custom: prefix\. 

**ID Token Signature**  
The signature of the ID token is calculated based on the header and payload of the JWT token\. When used outside of an application in your web APIs, you must always verify this signature before accepting the token\. See Verifying a JSON Web Token\. [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\.