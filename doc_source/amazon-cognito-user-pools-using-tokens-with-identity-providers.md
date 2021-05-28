# Using Tokens with User Pools<a name="amazon-cognito-user-pools-using-tokens-with-identity-providers"></a>

After a successful authentication, Amazon Cognito returns user pool tokens to your app\. You can use the tokens to grant your users access to your own server\-side resources or to the Amazon API Gateway\. Or you can exchange them for temporary AWS credentials to access other AWS services\. See [Common Amazon Cognito Scenarios](cognito-scenarios.md)\. 

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

User pool token handling and management for your web or mobile app is provided on the client side through Amazon Cognito SDKs\. For information on the SDKs, and sample code for JavaScript, Android, and iOS see [Amazon Cognito User Pool SDKs](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\. Many good libraries are available for decoding and verifying a JSON Web Token \(JWT\)\. Such libraries can help if you need to manually process tokens for server\-side API processing, or if you are using other programming languages\. See the [OpenID Foundation list of libraries for working with JWT tokens](http://openid.net/developers/jwt/)\.

Amazon Cognito user pools implements ID, access, and refresh tokens as defined by the OpenID Connect \(OIDC\) open standard:
+ The [ID token](http://openid.net/specs/openid-connect-core-1_0.html#IDToken) contains claims about the identity of the authenticated user such as `name`, `email`, and `phone_number`\.
+ The [access token](https://tools.ietf.org/html/rfc6749#section-1.4) contains scopes and groups and is used to grant access to authorized resources\.
+ The [refresh token](https://tools.ietf.org/html/rfc6749#section-1.5) contains the information necessary to obtain a new ID or access token\.

**Important**  
We strongly recommended that you secure all tokens in transit and storage in the context of your application\.

For more information on OIDC see the [OpenID Connect \(OIDC\) specification](http://openid.net/specs/openid-connect-core-1_0.html)\.

**Topics**
+ [Using the ID Token](#amazon-cognito-user-pools-using-the-id-token)
+ [Using the Access Token](#amazon-cognito-user-pools-using-the-access-token)
+ [Using the Refresh Token](#amazon-cognito-user-pools-using-the-refresh-token)
+ [Revoking All Tokens for a User](#amazon-cognito-identity-user-pools-revoking-all-tokens-for-user)
+ [Verifying a JSON Web Token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)

## Using the ID Token<a name="amazon-cognito-user-pools-using-the-id-token"></a>

The ID token is a [JSON Web Token \(JWT\)](https://tools.ietf.org/html/rfc7519) that contains claims about the identity of the authenticated user such as `name`, `email`, and `phone_number`\. You can use this identity information inside your application\. The ID token can also be used to authenticate users to your resource servers or server applications\. You can also use an ID token outside of the application with your web APIs\. In those cases, you must verify the signature of the ID token before you can trust any claims inside the ID token\. See [Verifying a JSON Web Token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md). You can set the ID token expiration to any value between 5 minutes and 1 day\. This value can be set per app client\.

**Important**  
If you use Hosted UI and setup tokens less than an hour, the end user will be able to get new tokens based on their session cookie which is currently fixed at one hour\.

### ID Token Header<a name="user-pool-id-token-header"></a>

The header contains two pieces of information: the key ID \(`kid`\), and the algorithm \(`alg`\)\.

```
{
"kid" : "1234example="
"alg" : "RS256",
}
```

**Key ID \(`kid`\)**  
The `kid` parameter is a hint that indicates which key was used to secure the JSON Web Signature \(JWS\) of the token\.  
For more information about the kid parameter, see the [Key Identifier \(kid\) Header Parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.5)\.

**Algorithm \(`alg`\)**  
The `alg` parameter represents the cryptographic algorithm that is used to secure the ID token\. User pools use an RS256 cryptographic algorithm, which is an RSA signature with SHA\-256\.  
For more information about the `alg` parameter, see [Algorithm \(alg\) Header Parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.4)\.

### ID Token Payload<a name="user-pool-id-token-payload"></a>

This is a sample payload from an ID token\. It contains claims about the authenticated user\. For more information about OIDC standard claims, see the [OIDC Standard Claims](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\.

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
      "email": "janedoe@example.com"
      }
```

**Subject \(`sub`\)**  
The `sub` claim is a unique identifier \(UUID\) for the authenticated user\. It is not the same as the user name which may not be unique\.

**Issuer \(`iss`\)**  
The `iss` claim has the following format:  
https://cognito\-idp\.\{region\}\.amazonaws\.com/\{userPoolId\}\.  
For example, suppose you created a user pool in the `us-east-1` Region and its user pool ID is `u123456`\. In that case, the ID token that is issued for users of your user pool have the following `iss` claim value:  
https://cognito\-idp\.us\-east\-1\.amazonaws\.com/u123456\.

**Audience \(`aud`\)**  
The `aud` claim contains the `client_id` that is used in the user authenticated\.

**Token use \(`token_use`\)**  
`The token_use` claim describes the intended purpose of this token\. Its value is always `id` in the case of the ID token\.

**Authentication time \(`auth_time`\)**  
The `auth_time` claim contains the time when the authentication occurred\. Its value is a JSON number representing the number of seconds from 1970\-01\-01T0:0:0Z as measured in UTC format\. On refreshes, it represents the time when the original authentication occurred, not the time when the token was issued\.

The ID token can contain OpenID Connect \(OIDC\) standard claims that are defined in [OIDC Standard Claims](http://openid.net/specs/openid-connect-core-1_0.html#Claims)\. It can also contain custom attributes that you define in your user pool\.

**Note**  
User pool custom attributes are always prefixed with a `custom:` prefix\. 

### ID Token Signature<a name="user-pool-id-token-signature"></a>

The signature of the ID token is calculated based on the header and payload of the JWT token\. When used outside of an application in your web APIs, you must always verify this signature before accepting the token\. See [Verifying a JSON Web Token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\.

## Using the Access Token<a name="amazon-cognito-user-pools-using-the-access-token"></a>



The user pool access token contains claims about the authenticated user, a list of the user's groups, and a list of scopes\. The purpose of the access token is to authorize API operations in the context of the user in the user pool\. For example, you can use the access token to grant your user access to add, change, or delete user attributes\.

 You can also use the access token to make access control decisions and authorize operations for your users based on scopes or groups\. 



The access token is represented as a JSON Web Token \(JWT\)\. The header for the access token has the same structure as the ID token\. However, the key ID \(`kid`\) is different because different keys are used to sign ID tokens and access tokens\. As with the ID token, you must first verify the signature of the access token in your web APIs before you can trust any of its claims\. See [Verifying a JSON Web Token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md) You can set the access token expiration to any value between 5 minutes and 1 day\. This value can be set per app client\.

**Important**  
For access and ID tokens, don't specify a minimum less than an hour\. Amazon Cognito HostedUI uses cookies that are valid for an hour, if you enter a minimum less than an hour, you won't get a lower expiry time\.

### Access Token Header<a name="user-pool-access-token-header"></a>

The header contains two pieces of information: the key ID \(`kid`\), and the algorithm \(`alg`\)\.

```
{
"kid" : "1234example="
"alg" : "RS256",
}
```

**Key ID \(`kid`\)**  
The `kid` parameter is a hint indicating which key was used to secure the JSON Web Signature \(JWS\) of the token\.  
For more information about the `kid` parameter, see the [Key Identifier \(kid\) Header Parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.5)\.

**Algorithm \(`alg`\)**  
The `alg` parameter represents the cryptographic algorithm that was used to secure the access token\. User pools use an RS256 cryptographic algorithm, which is an RSA signature with SHA\-256\.  
For more information about the `alg` parameter, see [Algorithm \(alg\) Header Parameter](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-41#section-4.4)\.

### Access Token Payload<a name="user-pool-access-token-payload"></a>

This is a sample payload from an access token\. For more information, see [JWT Claims](https://tools.ietf.org/html/rfc7519#section-4)\.

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
The `scope` claim is a list of Oauth 2\.0 scopes that define what access the token provides\.

**Authentication time \(`auth_time`\)**  
The `auth_time` claim contains the time when the authentication occurred\. Its value is a JSON number that represents the number of seconds from 1970\-01\-01T0:0:0Z as measured in UTC format\. On refreshes, it represents the time when the original authentication occurred, not the time when the token was issued\.

**Issuer \(`iss`\)**  
The `iss` claim has the following format:  
https://cognito\-idp\.\{region\}\.amazonaws\.com/\{userPoolId\}\.

### Access Token Signature<a name="user-pool-access-token-signature"></a>

The signature of the access token is calculated based on the header and payload of the JWT token\. When used outside of an application in your web APIs, you must always verify this signature before accepting the token\. See [Verifying a JSON Web Token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)\.

## Using the Refresh Token<a name="amazon-cognito-user-pools-using-the-refresh-token"></a>

You can use the refresh token to retrieve new ID and access tokens\.

By default, the refresh token expires 30 days after your app user signs in to your user pool\. When you create an app for your user pool, you can set the app's refresh token expiration to any value between 60 minutes and 10 years\.

The Mobile SDK for iOS and the Mobile SDK for Android automatically refresh your ID and access tokens if there is a valid \(unexpired\) refresh token present\. The ID and access tokens have a minimum remaining validity of 5 minutes\. If the refresh token is expired, your app user must reauthenticate by signing in again to your user pool\. If the minimum for the access token and ID token is set to 5 minutes, and you are using the SDK, the refresh token will continually refresh\. To see the expected behavior, set a minimum of 7 minutes instead of 5 minutes\.

**Note**  
The Mobile SDK for Android offers the option to change the minimum validity period of the ID and access tokens to a value between 0 and 30 minutes\. See the `setRefreshThreshold()` method of [CognitoIdentityProviderClientConfig](https://docs.aws.amazon.com/AWSAndroidSDK/latest/javadoc/com/amazonaws/mobileconnectors/cognitoidentityprovider/util/CognitoIdentityProviderClientConfig.html) in the AWS Mobile SDK for Android API Reference\.

Your user's account itself never expires, as long as the user has logged in at least once before the `UnusedAccountValidityDays` time limit for new accounts\.

To use the refresh token to get new ID and access tokens with the user pool API, use the [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) or [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) methods\. Pass `REFRESH_TOKEN_AUTH` for the `AuthFlow` parameter\. The authorization parameters, `AuthParameters`, are a key\-value map where the key is `"REFRESH_TOKEN"` and the value is the actual refresh token\. Amazon Cognito responds with new ID and access tokens\.

## Revoking All Tokens for a User<a name="amazon-cognito-identity-user-pools-revoking-all-tokens-for-user"></a>

Users can sign out from all devices where they are currently signed in when you revoke all of the user's tokens by using the `GlobalSignOut` and `AdminUserGlobalSignOut` API operations\. After the user has been signed out, the following happens:
+ The user's refresh token cannot be used to get new tokens for the user\.
+ The user's access token cannot be used for the user pools service\.
+ The user must reauthenticate to get new tokens\.

An app can use the `GlobalSignOut` API to allow individual users to sign themselves out from all devices\. Typically an app would present this option as a choice, such as **Sign out from all devices**\. The app must call this method with the user's valid, nonexpired, nonrevoked access token\. This method cannot be used to allow a user to sign out another user\.

An administrator app can use the `AdminUserGlobalSignOut` API to allow administrators to sign out a user from all devices\. The administrator app must call this method with AWS developer credentials and pass the user pool ID and the user's user name as parameters\. The `AdminUserGlobalSignOut` API can sign out any user in the user pool\.
