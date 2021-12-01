# Authentication with a user pool<a name="authentication"></a>

Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\.

After successful authentication, Amazon Cognito returns user pool tokens to your app\. You can use the tokens to grant your users access to your own server\-side resources, or to the Amazon API Gateway\. Or, you can exchange them for AWS credentials to access other AWS services\.

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

User pool token handling and management for your web or mobile app is provided on the client side through Amazon Cognito SDKs\. Likewise, the Mobile SDK for iOS and the Mobile SDK for Android automatically refresh your ID and access tokens if there is a valid \(non\-expired\) refresh token present, and the ID and access tokens have a minimum remaining validity of 5 minutes\. For information on the SDKs, and sample code for JavaScript, Android, and iOS see [Amazon Cognito user pool SDKs](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

After your app user successfully signs in, Amazon Cognito creates a session and returns an ID, access, and refresh token for the authenticated user\.

------
#### [ JavaScript ]

```
// Amazon Cognito creates a session which includes the id, access, and refresh tokens of an authenticated user.

var authenticationData = {
        Username : 'username',
        Password : 'password',
    };
    var authenticationDetails = new AmazonCognitoIdentity.AuthenticationDetails(authenticationData);
    var poolData = { UserPoolId : 'us-east-1_ExaMPle',
        ClientId : '1example23456789'
    };
    var userPool = new AmazonCognitoIdentity.CognitoUserPool(poolData);
    var userData = {
        Username : 'username',
        Pool : userPool
    };
    var cognitoUser = new AmazonCognitoIdentity.CognitoUser(userData);
    cognitoUser.authenticateUser(authenticationDetails, {
        onSuccess: function (result) {
            var accessToken = result.getAccessToken().getJwtToken();

            /* Use the idToken for Logins Map when Federating User Pools with identity pools or when passing through an Authorization Header to an API Gateway Authorizer */
            var idToken = result.idToken.jwtToken;
        },

        onFailure: function(err) {
            alert(err);
        },

});
```

------
#### [ Android ]

```
// Session is an object of the type CognitoUserSession, and includes the id, access, and refresh tokens for a user.

String idToken = session.getIdToken().getJWTToken();
String accessToken = session.getAccessToken().getJWT();
```

------
#### [ iOS \- swift ]

```
// AWSCognitoIdentityUserSession includes id, access, and refresh tokens for a user.

- (AWSTask<AWSCognitoIdentityUserSession *> *)getSession;
```

------
#### [ iOS \- objective\-C ]

```
// AWSCognitoIdentityUserSession includes the id, access, and refresh tokens for a user.

[[user getSession:@"username" password:@"password" validationData:nil scopes:nil] continueWithSuccessBlock:^id _Nullable(AWSTask<AWSCognitoIdentityUserSession *> * _Nonnull task) {
    // success, task.result has user session
    return nil;
}];
```

------

**Topics**
+ [User pool authentication flow](amazon-cognito-user-pools-authentication-flow.md)