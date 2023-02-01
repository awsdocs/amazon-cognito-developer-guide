# User pool OIDC and hosted UI API endpoints reference<a name="cognito-userpools-server-contract-reference"></a>

Amazon Cognito activates the public webpages listed here when you assign a domain to your user pool\. Your domain serves as a central access point for all of your app clients\. They include the hosted UI, where your users can sign up and sign in \(the [Login endpoint](login-endpoint.md)\), and sign out \(the [Logout endpoint](logout-endpoint.md)\)\. For more information about these resources, see [Using the Amazon Cognito hosted UI for sign\-up and sign\-in](cognito-user-pools-app-integration.md)\.

**Warning**  
Don't pin the end\-entity or intermediate transport layer security \(TLS\) certificates for Amazon Cognito domains\. AWS manages all certificates for all of your user pool endpoints and prefix domains\. The certificate authorities \(CAs\) in the chain of trust that supports Amazon Cognito certificates dynamically rotate and renew\. When you pin your app to an intermediate or leaf certificate, your app can fail without notice when AWS rotates certificates\.  
Instead, pin your application to all available [Amazon root certificates](https://www.amazontrust.com/repository/)\. For more information, see best practices and recommendations at [Certificate pinning](https://docs.aws.amazon.com/acm/latest/userguide/acm-bestpractices.html#best-practices-pinning) in the *AWS Certificate Manager User Guide*\.

These pages also include the public web resources that allow your user pool to communicate with third\-party SAML, OpenID Connect \(OIDC\) and OAuth 2\.0 identity providers \(IdPs\)\. To sign in a user with a federated identity provider, your users must initiate a request to the [Login endpoint](login-endpoint.md) or the [Authorize endpoint](authorization-endpoint.md)\. Your app can also sign in *native users* with the [Amazon Cognito API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html)\. Native users are those that you created or who signed up in your user pool\.

In addition to the OAuth 2\.0 REST API endpoints, Amazon Cognito also integrates with SDKs for Android, iOS, JavaScript, and more\. The SDKs provide tools to interact both with your user pool API endpoints and Amazon Cognito API service endpoints\. For more information about service endpoints, see [Amazon Cognito Identity endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html)\.

Along with the endpoints in this guide, Amazon Cognito creates the following endpoints when you assign a domain to your user pool\.


**Additional user pool endpoints**  

| Endpoint URL | Description | Category | 
| --- | --- | --- | 
| https://cognito\-idp\.Region\.amazonaws\.com/your user pool ID/\.well\-known/openid\-configuration | A directory of the OIDC architecture of your user pool\. | OIDC | 
| https://cognito\-idp\.Region\.amazonaws\.com/your user pool ID/\.well\-known/jwks\.json | Public keys that you can use to validate Amazon Cognito tokens\. | OIDC | 
| https://Your user pool domain/oauth2/idpresponse | Social identity providers must redirect your users to this endpoint with an authorization code\. Amazon Cognito redeems the code for a token when it authenticates your federated user\. | OAuth 2\.0 & OIDC | 
| https://Your user pool domain/saml2/idpresponse | To authenticate your SAML 2\.0 federated user, your identity provider must redirect your users to this endpoint with a SAML response\. | SAML 2\.0 | 
| https://Your user pool domain/confirmUser | Confirms users who have selected an email link to verify their user account\. | Hosted UI | 
| https://Your user pool domain/signup | Signs up a new user\. The /login page directs your user to /signup when they select Sign up\. | Hosted UI | 
| https://Your user pool domain/forgotPassword | Prompts your user for their user name and sends a password\-reset code\. The /login page directs your user to /forgotPassword when they select Forgot your password?\. | Hosted UI | 
| https://Your user pool domain/confirmforgotPassword | Prompts your user for their password\-reset code and a new password\. The /forgotPassword page directs your user to /confirmforgotPassword when they select Reset your password\. | Hosted UI | 

For more information on the OpenID and OAuth standards, see [OpenID Connect 1\.0](http://openid.net/specs/openid-connect-core-1_0.html) and [OAuth 2\.0](https://tools.ietf.org/html/rfc6749)\.

**Topics**
+ [Authorize endpoint](authorization-endpoint.md)
+ [Token endpoint](token-endpoint.md)
+ [UserInfo endpoint](userinfo-endpoint.md)
+ [Login endpoint](login-endpoint.md)
+ [Logout endpoint](logout-endpoint.md)
+ [Revoke endpoint](revocation-endpoint.md)