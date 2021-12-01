# Configuring app client settings<a name="cognito-user-pools-app-settings"></a>

------
#### [ Original console ]

**Note**  
The **General settings** tab only appears when you're editing an existing user pool\.

On the **General settings** tab, you must configure at least one identity provider \(IdP\) for your apps if you want to use the built\-in hosted pages to sign up and sign in users, or if you want to use OAuth2\.0 flows\. For more information, see [Configuring a user pool app client](cognito-user-pools-app-idp-settings.md)\.

**To specify app client settings for your user pool**

1. In **Enabled Identity Providers**, select the identity providers you want to use for the apps that you configured in the **App Clients** tab\.

1. Enter the **Callback URLs** you want, separated by commas\. These URLs apply to all of the selected identity providers\.
**Note**  
You must register the URLs in the console, or by using the AWS CLI or API, before you can use them in your app\.

1. Enter the **Sign out URLs** you want, separated by commas\.
**Note**  
You must register the URLs in the console, or by using the CLI or API, before you can use them in your app\.

1. Under **OAuth 2\.0**, select the from the following options\. For more information, see [App client settings terminology](cognito-user-pools-app-idp-settings.md#cognito-user-pools-app-idp-settings-about) and the [OAuth 2\.0 specification](https://oauth.net/2/)\.
   + For **Allowed OAuth Flows**, select **Authorized code grant** and **Implicit grant**\. Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.
   + For **Allowed OAuth Scopes**, select the scopes you want\. Each scope is a set of one or more standard attributes\.
   + For **Allowed Custom Scopes**, select the scopes you want from any custom scopes that you have defined\. Custom scopes are defined in the **Resource Servers** tab\. For more information, see [Defining resource servers for your user pool](cognito-user-pools-define-resource-servers.md)\.

------
#### [ New console ]

On the **Sign\-in experience** tab, you must configure at least one **Federated identity provider sign\-in** identity provider \(IdP\) if you want to use the built\-in hosted pages to sign up and sign in users, or if you want to use OAuth2\.0 flows\. For more information, see [Configuring a user pool app client](cognito-user-pools-app-idp-settings.md)\.

**Configure the app**

1. In the **App integration** tab, select your app client under **App clients**\. Review your current **Hosted UI** information\.

1. **Add a callback URL** under **Allowed callback URL\(s\)**\. A callback URL is where the user is redirected to after a successful sign\-in\.

1. **Add a sign\-out URL** under **Allowed sign\-out URL\(s\)**\. A sign\-out URL is where your user is redirected to after signing out\.

1. Add at least one from the list of **Identity providers**\.

1. Under **OAuth 2\.0 grant types**, select **Authorization code grant** to return an authorization code that is then exchanged for user pool tokens\. Because the tokens are never exposed directly to an end user, they are less likely to become compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\. For security reasons, we recommend that you use the authorization code grant flow, together with [Proof key for code Exchange \(PKCE\)](https://tools.ietf.org/html/rfc7636), for mobile apps\.

1. Under **OAuth 2\.0 grant types**, select **Implicit grant** to have user pool JSON web tokens \(JWT\) returned to you from Amazon Cognito\. You can use this flow when there's no backend available to exchange an authorization code for tokens\. It's also helpful for debugging tokens\.

1. You can enable both the **Authorization code** and the **Implicit code** grants, and then use each grant as needed\. If neither **Authorization code** or **Implicit code** grants are selected and your app client has a client secret, you can enable **Client credentials** grants\. Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.

1. Select the **OpenID Connect scopes** that you want to authorize for this app client\.

1. Choose **Save changes**\.

------