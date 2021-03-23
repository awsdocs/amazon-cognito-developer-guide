# Using the Amazon Cognito Hosted UI for Sign\-Up and Sign\-In<a name="cognito-user-pools-app-integration"></a>

Amazon Cognito Hosted UI provides you an OAuth 2\.0 compliant authorization server\. It provides default implementation of end user flows such as registration, authentication etc\. You can customize the user flows such as addition of Multi Factor Authentication \(MFA\) by simply changing your user pool configuration\. Your application will redirect to Hosted UI and it will handle the user flows\. The user experience can be customized by providing brand specific logos and changing the look and feel\. Amazon Cognito Hosted UI also allows you to easily add ability for end users to sign in with social providers \(Facebook, LoginWithAmazon, Google and Apple\), any OpenID Connect \(OIDC\) compliant and SAML providers\. 

**Topics**
+ [Setting Up the Hosted UI with AWS Amplify](#cognito-user-pools-app-integration-amplify)
+ [Setting Up the Hosted UI with the Amazon Cognito Console](#cognito-user-pools-create-an-app-integration)
+ [Configuring a User Pool App Client](cognito-user-pools-app-idp-settings.md)
+ [Configuring a User Pool Domain](cognito-user-pools-assign-domain.md)
+ [Customizing the Built\-in Sign\-in and Sign\-up Webpages](cognito-user-pools-app-ui-customization.md)
+ [Defining Resource Servers for Your User Pool](cognito-user-pools-define-resource-servers.md)

## Setting Up the Hosted UI with AWS Amplify<a name="cognito-user-pools-app-integration-amplify"></a>

If you use AWS Amplify to add authentication to your web or mobile app, you can set up your hosted UI by using the command line interface \(CLI\) and libraries in the AWS Amplify framework\. To add authentication to your app, you use the AWS Amplify CLI to add the Auth category to your project\. Then, in your client code, you use the AWS Amplify libraries to authenticate users with your Amazon Cognito user pool\.

You can display a pre\-built hosted UI, or you can federate users through an OAuth 2\.0 endpoint that redirects to a social sign\-in provider, such as Facebook, Google, Amazon, or Apple\. After a user successfully authenticates with the social provider, AWS Amplify creates a new user in your user pool if needed, and it provides the user's OIDC token to you app\.

For more information, see the AWS Amplify framework documentation for your app platform:
+ [AWS Amplify authentication for JavaScript\.](https://aws-amplify.github.io/docs/js/authentication)
+ [AWS Amplify authentication for iOS\.](https://aws-amplify.github.io/docs/ios/authentication)
+ [AWS Amplify authentication for Android\.](https://aws-amplify.github.io/docs/android/authentication)

## Setting Up the Hosted UI with the Amazon Cognito Console<a name="cognito-user-pools-create-an-app-integration"></a>

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the navigation bar on the left\-side of the page, choose **App clients** under **General settings**\.

1. Choose **Add an app client**\.

1. Enter your app name\.

1. Unless required by your authorization flow, clear the option **Generate client secret**\. The client secret is used by applications that have a server\-side component that can secure the client secret\.

1. \(Optional\) Change the token expiration settings\.

1. Select **Auth Flows Configuration** options\.

1. Choose a **Security configuration**\. We recommend you select **Enabled**\.

1. \(Optional\) Choose **Set attribute read and write permissions**\.

1. Choose **Create app client**\.

1. Note the **App client id**\.

1. Choose **Return to pool details**\.

**Configure the app**

1. Choose **App client settings** from the navigation bar on the left\-side of the console page\.

1. Select **Cognito User Pool** as one of the **Enabled Identity Providers**\.
**Note**  
To sign in with external identity providers \(IdPs\), such as Facebook, Amazon, Google, or Apple as well as through OpenID Connect \(OIDC\) or SAML IdPs, first configure them as described next, and then return to the **App client settings** page to enable them\.

1. Enter **Callback URL\(s\)**\. A callback URL indicates where the user is to be redirected after a successful sign\-in\.

1. Enter **Sign out URL\(s\)**\. A sign\-out URL indicates where your user is to be redirected after signing out\.

1. Select **Authorization code grant** to return an authorization code that is then exchanged for user pool tokens\. Because the tokens are never exposed directly to an end user, they are less likely to become compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\. For security reasons, we recommend that you use the authorization code grant flow, together with [Proof Key for Code Exchange \(PKCE\)](https://tools.ietf.org/html/rfc7636), for mobile apps\.

1. Select **Implicit grant** to have user pool JSON web tokens \(JWT\) returned to you from Amazon Cognito\. You can use this flow when there's no backend available to exchange an authorization code for tokens\. It's also helpful for debugging tokens\.

1. You can enable both the **Authorization code grant** and the **Implicit code grant**, and then use each grant as needed\.

1. Unless you specifically want to exclude one, select the check boxes for all of the **Allowed OAuth scopes**\.

1. Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.

1. Choose **Save changes**\.

**Configure a domain**

1. Select **Choose domain name**\.

1. On the **Domain name** page, type a domain prefix and check availability or enter your own domain\.

1. Make a note of the complete domain address\.

1. Choose **Save changes**\.

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

You can decode and verify user pool tokens using AWS Lambda, see [Decode and verify Amazon Cognito JWT tokens](https://github.com/awslabs/aws-support-tools/tree/master/Cognito/decode-verify-jwt) on the AWS GitHub website\.

Amazon Cognito user pools tokens are signed using an RS256 algorithm\.

You might have to wait a minute to refresh your browser before changes you made in the console appear\.

Your domain is shown on the **Domain name** page\. Your app client ID and callback URL are shown on the **App client settings** page\.

**Note**  
The Amazon Cognito hosted sign\-in web page does not support the custom authentication flow\.