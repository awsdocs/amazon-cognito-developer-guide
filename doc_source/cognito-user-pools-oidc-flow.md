# OIDC user pool IdP authentication flow<a name="cognito-user-pools-oidc-flow"></a>

When your user signs in to your application using an OIDC IdP, they pass through the following authentication flow\.

1. Your user lands on the Amazon Cognito built\-in sign\-in page, and is offered the option to sign in through an OIDC IdP such as Salesforce\.

1. Your user is redirected to the `authorization` endpoint of the OIDC IdP\.

1. After your user is authenticated, the OIDC IdP redirects to Amazon Cognito with an authorization code\.

1. Amazon Cognito exchanges the authorization code with the OIDC IdP for an access token\.

1. Amazon Cognito creates or updates the user account in your user pool\. 

1. Amazon Cognito issues your application bearer tokens, which might include identity, access, and refresh tokens\.

![\[User pool OIDC IdP authentication flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User pool OIDC IdP authentication flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User pool OIDC IdP authentication flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Amazon Cognito cancels authentication requests that do not complete within 5 minutes, and redirects the user to the hosted UI\. The page displays a `Something went wrong` error message\.

OIDC is an identity layer on top of OAuth 2\.0, which specifies JSON\-formatted \(JWT\) identity tokens that are issued by IdPs to OIDC client apps \(relying parties\)\. See the documentation for your OIDC IdP for information about to add Amazon Cognito as an OIDC relying party\.

When a user authenticates, the user pool returns ID, access, and refresh tokens\. The ID token is a standard [OIDC](http://openid.net/specs/openid-connect-core-1_0.html) token for identity management, and the access token is a standard [OAuth 2\.0](https://oauth.net/2/) token\.