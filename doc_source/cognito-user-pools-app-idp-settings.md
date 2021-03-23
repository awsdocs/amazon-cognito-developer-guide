# Configuring a User Pool App Client<a name="cognito-user-pools-app-idp-settings"></a>

After you create a user pool, you can configure an app client to use the built\-in webpages for signing up and signing in your users\. For terminology, see [App Client Settings Terminology](#cognito-user-pools-app-idp-settings-about)\.

## App Client Settings Terminology<a name="cognito-user-pools-app-idp-settings-about"></a>

The following terms and definitions can help you with configuring your app client\.

**Enabled Identity Providers**  
You can choose your identity provider \(IDP\) to authenticate your users\. This service can be performed by your user pool, or by a third party such as Facebook\. Before you can use an IdP, you need to enable it\. You can enable multiple IdPs, but you must enable at least one\. For more information on using external IdPs see [Adding User Pool Sign\-in Through a Third Party](cognito-user-pools-identity-federation.md)\.

**Callback URL\(s\)**  
A callback URL indicates where the user is to be redirected after a successful sign\-in\. Choose at least one callback URL, and it should:  
+ Be an absolute URI\.
+ Be pre\-registered with a client\.
+ Not include a fragment component\.
See [OAuth 2\.0 \- Redirection Endpoint](https://tools.ietf.org/html/rfc6749#section-3.1.2)\.  
Amazon Cognito requires `HTTPS` over `HTTP` except for `http://localhost` for testing purposes only\.  
App callback URLs such as `myapp://example` are also supported\.

**Sign out URL\(s\)**  
A sign\-out URL indicates where your user is to be redirected after signing out\.

**Allowed OAuth Flows**  
The **Authorization code grant** flow initiates a code grant flow, which provides an authorization code as the response\. This code can be exchanged for access tokens with the [TOKEN Endpoint](token-endpoint.md)\. Because the tokens are never exposed directly to an end user, they are less likely to become compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\.  
For security reasons, we highly recommend that you use only the **Authorization code grant** flow, together with [PKCE](https://tools.ietf.org/html/rfc7636), for mobile apps\.
The **Implicit grant** flow allows the client to get the access token \(and, optionally, ID token, based on scopes\) directly from the [AUTHORIZATION Endpoint](authorization-endpoint.md)\. Choose this flow if your app cannot initiate the **Authorization code grant** flow\. For more information, see the [OAuth 2\.0 specification](https://oauth.net/2/)\.  
You can enable both the **Authorization code grant** and the **Implicit code grant**, and then use each grant as needed\.  
The **Client credentials** flow is used in machine\-to\-machine communications\. With it you can request an access token to access your own resources\. Use this flow when your app is requesting the token on its own behalf, not on behalf of a user\.  
Since the client credentials flow is not used on behalf of a user, only custom scopes can be used with this flow\. A custom scope is one that you define for your own resource server\. See [Defining Resource Servers for Your User Pool](cognito-user-pools-define-resource-servers.md)\.

**Allowed OAuth Scopes**  
Choose one or more of the following `OAuth` scopes to specify the access privileges that can be requested for access tokens\.  
+ The `phone` scope grants access to the `phone_number` and `phone_number_verified` claims\. This scope can only be requested with the `openid` scope\.
+ The `email` scope grants access to the `email` and `email_verified` claims\. This scope can only be requested with the `openid` scope\.
+ The `openid` scope returns all user attributes in the ID token that are readable by the client\. The ID token is not returned if the `openid` scope is not requested by the client\.
+ The `aws.cognito.signin.user.admin` scope grants access to [Amazon Cognito User Pool API operations](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html) that require access tokens, such as [UpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html) and [VerifyUserAttribute](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifyUserAttribute.html)\.
+ The `profile` scope grants access to all user attributes that are readable by the client\. This scope can only be requested with the `openid` scope\.

**Allowed Custom Scopes**  
A custom scope is one that you define for your own resource server in the **Resource Servers**\. The format is *resource\-server\-identifier*/*scope*\. See [Defining Resource Servers for Your User Pool](cognito-user-pools-define-resource-servers.md)\.

For more information about OAuth scopes, see the list of [standard OIDC scopes](http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)\.