# Configuring App Client Settings<a name="cognito-user-pools-app-settings"></a>

**Note**  
The **General settings** tab appears only when you're editing an existing user pool\.

On the **General settings** tab, you must configure at least one identity provider \(IdP\) for your apps if you want to use the built\-in hosted pages to sign up and sign in users or you want to use OAuth2\.0 flows\. For more information, see [Configuring a User Pool App Client](cognito-user-pools-app-idp-settings.md)\.

**To specify app client settings for your user pool**

1. In **Enabled Identity Providers**, select the identity providers you want for the apps that you configured in the **App Clients** tab\.

1. Enter the **Callback URLs** you want, separated by commas\. These URLs apply to all of the selected identity providers\.
**Note**  
You must register the URLs in the console, or by using the CLI or API, before you can use them in your app\.

1. Enter the **Sign out URLs** you want, separated by commas\.
**Note**  
You must register the URLs in the console, or by using the CLI or API, before you can use them in your app\.

1. Under **OAuth 2\.0**, select the from the following options\. For more information, see [App Client Settings Terminology](cognito-user-pools-app-idp-settings.md#cognito-user-pools-app-idp-settings-about) and the [OAuth 2\.0 specification](https://oauth.net/2/)\.
   + For **Allowed OAuth Flows**, select **Authorized code grant** and **Implicit grant**\. Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.
   + For **Allowed OAuth Scopes**, select the scopes you want\. Each scope is a set of one or more standard attributes\.
   + For **Allowed Custom Scopes**, select the scopes you want from any custom scopes that you have defined\. Custom scopes are defined in the **Resource Servers** tab\. For more information, see [Defining Resource Servers for Your User Pool](cognito-user-pools-define-resource-servers.md)\.