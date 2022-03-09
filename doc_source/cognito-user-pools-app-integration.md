# Using the Amazon Cognito hosted UI for sign\-up and sign\-in<a name="cognito-user-pools-app-integration"></a>

The Amazon Cognito Hosted UI provides you an OAuth 2\.0 compliant authorization server\. It includes default implementation of end user flows such as registration and authentication\. You can also customize user flows, such as the addition of Multi Factor Authentication \(MFA\), by changing your user pool configuration\. Your application will redirect to the Hosted UI,which will handle your user flows\. The user experience can be customized by providing brand\-specific logos, as well as customizing the design of Hosted UI elements\. The Amazon Cognito Hosted UI also allows you to add the ability for end users to sign in with social providers \(Facebook, Amazon, Google, and Apple\), as well as any OpenID Connect \(OIDC\)\-compliant and SAML providers\. 

**Topics**
+ [Setting up the hosted UI with AWS Amplify](#cognito-user-pools-app-integration-amplify)
+ [Setting up the hosted UI with the Amazon Cognito console](#cognito-user-pools-create-an-app-integration)
+ [Configuring a user pool app client](cognito-user-pools-app-idp-settings.md)
+ [Configuring a user pool domain](cognito-user-pools-assign-domain.md)
+ [Customizing the built\-in sign\-in and sign\-up webpages](cognito-user-pools-app-ui-customization.md)
+ [Defining resource servers for your user pool](cognito-user-pools-define-resource-servers.md)

## Setting up the hosted UI with AWS Amplify<a name="cognito-user-pools-app-integration-amplify"></a>

If you use AWS Amplify to add authentication to your web or mobile app, you can set up your hosted UI by using the command line interface \(CLI\) and libraries in the AWS Amplify framework\. To add authentication to your app, you use the AWS Amplify CLI to add the `Auth` category to your project\. Then, in your client code, you use the AWS Amplify libraries to authenticate users with your Amazon Cognito user pool\.

You can display a pre\-built hosted UI, or you can federate users through an OAuth 2\.0 endpoint that redirects to a social sign\-in provider, such as Facebook, Google, Amazon, or Apple\. After a user successfully authenticates with the social provider, AWS Amplify creates a new user in your user pool if needed, and then provides the user's OIDC token to your app\.

For more information, see the AWS Amplify framework documentation for your app platform:
+ [AWS Amplify authentication for JavaScript\.](https://aws-amplify.github.io/docs/js/authentication)
+ [AWS Amplify authentication for iOS\.](https://aws-amplify.github.io/docs/ios/authentication)
+ [AWS Amplify authentication for Android\.](https://aws-amplify.github.io/docs/android/authentication)

## Setting up the hosted UI with the Amazon Cognito console<a name="cognito-user-pools-create-an-app-integration"></a>

------
#### [ Original console ]

**Create an app client**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the navigation bar on the left\-side of the page, under **General settings**, choose **App clients** \.

1. Choose **Add an app client**\.

1. Enter a name for your app\.

1. Unless required by your authorization flow, clear the option **Generate client secret**\. The client secret is used by applications that have a server\-side component that can secure the client secret\.

1. \(Optional\) Change the token expiration settings\.

1. Select **Auth Flows Configuration** options\. For more information, see [User Pool Authentication Flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html)\.

1. Choose a **Security configuration**\. We recommend you select **Enabled**\.

1. \(Optional\) Choose **Set attribute read and write permissions**\. For more information, see [Attribute Permissions and Scopes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-attribute-permissions-and-scopes)\.

1. Choose **Create app client**\.

1. Note the **App client id**\.

1. Choose **Return to pool details**\.

**Configure the app**

1. Choose **App client settings** from the navigation bar on the leftside of the console page\.

1. Select **Cognito User Pool** as one of the **Enabled Identity Providers**\.
**Note**  
To sign in with external identity providers \(IdPs\), such as Facebook, Amazon, Google, or Apple, as well as through OpenID Connect \(OIDC\) or SAML IdPs, first configure them as described in the following steps, and then return to the **App client settings** page to enable them\.

1. Enter **Callback URL\(s\)**\. A callback URL indicates where the user will be redirected after a successful sign\-in\.

1. Enter **Sign out URL\(s\)**\. A sign\-out URL indicates where your user will be redirected after signing out\.

1. Select **Authorization code grant** to return an authorization code that is then exchanged for user pool tokens\. Because the tokens are never exposed directly to an end user, they are less likely to be compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\. For security reasons, we recommend that you use the authorization code grant flow, together with [Proof key for code Exchange \(PKCE\)](https://tools.ietf.org/html/rfc7636), for mobile apps\.

1. Select **Implicit grant** to have user pool JSON web tokens \(JWT\) returned to you from Amazon Cognito\. You can use this flow when there's no backend available to exchange an authorization code for tokens\. It's also helpful for debugging tokens\.

1. You can enable both the **Authorization code grant** and the **Implicit code grant**, and then use each grant as needed\.

1. Unless you specifically want to exclude one, select the check boxes for all of the **Allowed OAuth scopes**\.

1. Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.

1. Choose **Save changes**\.

**Configure a domain**

1. Select **Choose domain name**\.

1. On the **Domain name** page, type a domain prefix and check availability, or enter your own domain\.

1. Make a note of the complete domain address\.

1. Choose **Save changes**\.

------
#### [ New console ]

**Create an app client**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. YIf prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Select the **App integration** tab\.

1. Under **App clients**, select **Create an app client**\.

1. Select an **App type**: **Public client**, **Confidential client**, or **Other**\. A **Public client** typically operates from your users' devices and uses unauthenticated and token\-authenticated APIs\. A **Confidential client** typically operates from an app on a central server that you trust with client secrets and API credentials, and uses authorization headers and AWS Identity and Access Management credentials to sign requests\. If your use case is different from the preconfigured app client settings for a **Public client** or a **Confidential client**, select **Other**\.

1. Enter an **App client name**\.

1. Select the **Authentication flows** you want to allow in your app client\.

1. \(Optional\) Configure token expiration\.

   1. Specify the **Refresh token expiration** for the app client\. The default value is 30 days\. You can change it to any value between 1 hour and 10 years\.

   1. Specify the **Access token expiration** for the app client\. The default value is 1 hour\. You can change it to any value between 5 minutes and 24 hours\.

   1. Specify the **ID token expiration** for the app client\. The default value is 1 hour\. You can change it to any value between 5 minutes and 24 hours\.
**Important**  
If you use the hosted UI and configure a token lifetime of less than an hour, your user will be able to use tokens based on their session cookie duration, which is currently fixed at one hour\.

1. Choose **Generate client secret** to have Amazon Cognito generate a client secret for you\. Client secrets are typically associated with confidential clients\.

1. Choose whether you will **Enable token revocation** for this app client\. This will increase the size of tokens\. For more information, see [Revoking Tokens](https://docs.aws.amazon.com/cognito/latest/developerguide/token-revocation.html)\.

1. Choose whether you will **Prevent error messages that reveal user existence** for this app client\. Amazon Cognito will respond to sign\-in requests for nonexistent users with a generic message stating that either the user name or password was incorrect\.

1. \(Optional\) Configure **Attribute read and write permissions** for this app client\. Your app client can have permission to read and write a limited subset of your user pool's attribute schema\.

1. Choose **Create**\.

1. Note the **Client id**\. This will identify the app client in sign\-up and sign\-in requests\.

**Configure the app**

1. In the **App integration** tab, select your app client under **App clients**\. Review your current **Hosted UI** information\.

1. **Add a callback URL** under **Allowed callback URL\(s\)**\. A callback URL is where the user is redirected to after a successful sign\-in\.

1. **Add a sign\-out URL** under **Allowed sign\-out URL\(s\)**\. A sign\-out URL is where your user is redirected to after signing out\.

1. Add at least one of the listed options from the list of **Identity providers**\.

1. Under **OAuth 2\.0 grant types**, select **Authorization code grant** to return an authorization code that is then exchanged for user pool tokens\. Because the tokens are never exposed directly to an end user, they are less likely to become compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\. For security reasons, we recommend that you use the authorization code grant flow, together with [Proof key for code Exchange \(PKCE\)](https://tools.ietf.org/html/rfc7636), for mobile apps\.

1. Under **OAuth 2\.0 grant types**, select **Implicit grant** to have user pool JSON web tokens \(JWT\) returned to you from Amazon Cognito\. You can use this flow when there's no backend available to exchange an authorization code for tokens\. It's also helpful for debugging tokens\.

1. You can enable both the **Authorization code** and the **Implicit code** grants, and then use each grant as needed\. If neither **Authorization code** or **Implicit code ** grants are selected and your app client has a client secret, you can enable **Client credentials** grants\. Select **Client credentials** only if your app needs to request access tokens on its own behalf and not on behalf of a user\.

1. Select the **OpenID Connect scopes** that you want to authorize for this app client\.

1. Choose **Save changes**\.

**Configure a domain**

1. Navigate to the **App integration** tab for your user pool\.

1. Next to **Domain**, choose **Actions** and select **Create custom domain** or **Create Cognito domain**\. If you have already configured a user pool domain, choose **Delete Cognito domain** or **Delete custom domain** before creating a new custom domain\.

1. Enter an available domain prefix to use with a **Cognito domain**\. For information on setting up a **Custom domain**, see [Using your own Domain for the hosted UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-add-custom-domain.html)

1. Choose **Create**\.

------

**To view your sign\-in page**

You can view the hosted UI sign\-in webpage with the following URL\. Note the `response_type`\. In this case, **response\_type=code** for the authorization code grant\.

```
https://<your_domain>/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>
```

You can view the hosted UI sign\-in webpage with the following URL for the implicit code grant where **response\_type=token**\. After a successful sign\-in, Amazon Cognito returns user pool tokens to your web browser's address bar\.

```
https://<your_domain>/login?response_type=token&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>
```

You can find the JSON web token \(JWT\) identity token after the `#idtoken=` parameter in the response\.

Here's a sample response from an implicit grant request\. Your identity token string will be much longer\.

```
https://www.example.com/#id_token=123456789tokens123456789&expires_in=3600&token_type=Bearer  
```

If changes to your hosted UI pages do not immediately appear, wait a few minutes and then refresh the page\. Amazon Cognito user pools tokens are signed using an RS256 algorithm\. You can decode and verify user pool tokens using AWS Lambda, see [Decode and verify Amazon Cognito JWT tokens](https://github.com/awslabs/aws-support-tools/tree/master/Cognito/decode-verify-jwt) on the AWS GitHub website\.



Your domain is shown on the **Domain name** page\. Your app client ID and callback URL are shown on the **App client settings** page\.

**Note**  
The Amazon Cognito hosted sign\-in web page does not support the custom authentication flow\.
