# User pool authentication and authorization endpoints reference<a name="cognito-userpools-server-contract-reference"></a>

Amazon Cognito creates the public webpages listed here when you set up domains for your app clients\. They include the hosted UI, where your users can sign up and sign in \(the [Login endpoint](login-endpoint.md)\), and sign out \(the [Logout endpoint](logout-endpoint.md)\)\. For more information about these resources, see [Using the Amazon Cognito hosted UI for sign\-up and sign\-in](cognito-user-pools-app-integration.md)\.

These pages also include the public web resources that allow your user pool to communicate with third\-party SAML, OpenID Connect \(OIDC\) and OAuth 2\.0 identity providers \(IdPs\)\. To sign in a user with a federated identity provider, your users must initiate a request to the [Login endpoint](login-endpoint.md) or the [Authorize endpoint](authorization-endpoint.md)\. Your app can also sign in *native users* with the [Amazon Cognito API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html)\. Native users are those that you created or who signed up in your user pool\.

In addition to the OAuth 2\.0 REST API endpoints, Amazon Cognito also integrates with SDKs for Android, iOS, JavaScript, and more\. The SDKs provide tools to interact both with your user pool API endpoints and Amazon Cognito API service endpoints\. For more information about service endpoints, see [Amazon Cognito Identity endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html)\.

Along with the endpoints in this guide, Amazon Cognito creates the following endpoints when you assign a domain to your user pool\.


**Additional user pool endpoints**  

| Endpoint URL | Description | 
| --- | --- | 
| https://cognito\-idp\.Region\.amazonaws\.com/your user pool ID/\.well\-known/openid\-configuration | A directory of the OIDC architecture of your user pool\. | 
| https://cognito\-idp\.Region\.amazonaws\.com/your user pool ID/\.well\-known/jwks\.json | Public keys that you can use to validate Amazon Cognito tokens\. | 
| https://Your user pool domain/oauth2/idpresponse | Social identity providers must redirect your users to this endpoint with an authorization code\. Amazon Cognito redeems the code for a token when it authenticates your federated user\. | 
| https://Your user pool domain/saml2/idpresponse | To authenticate your SAML 2\.0 federated user, your identity provider must redirect your users to this endpoint with a SAML response\. | 

For more information on the OpenID and OAuth standards, see [OpenID Connect 1\.0](http://openid.net/specs/openid-connect-core-1_0.html) and [OAuth 2\.0](https://tools.ietf.org/html/rfc6749)\.

**Topics**
+ [Authorize endpoint](authorization-endpoint.md)
+ [Token endpoint](token-endpoint.md)
+ [UserInfo endpoint](userinfo-endpoint.md)
+ [Login endpoint](login-endpoint.md)
+ [Logout endpoint](logout-endpoint.md)
+ [Revoke endpoint](revocation-endpoint.md)