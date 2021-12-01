# Step 2\. Add an app to enable the hosted web UI<a name="cognito-user-pools-configuring-app-integration"></a>

After you create a user pool, you can create an app to use the built\-in webpages for signing up and signing in your users\.

------
#### [ Original console ]

**To create an app in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the navigation bar on the left\-side of the page, choose **App clients** under **General settings**\.

1. Choose **Add an app client**\.

1. Enter a name for your app client\.

1. For this exercise, clear the option **Generate client secret**\. Using a client secret with client\-side authentication, such as the JavaScript used in this exercise, is not secure and not recommended for a production app client\. Client secrets should only be used by applications that have a server\-side authentication component so that it can secure the client secret\.

1. Choose **Create app client**\.

1. Note the **App client ID**\.

1. Choose **Return to pool details**\.

1. Choose **App client settings** from the navigation bar on the left\-side of the console page\.

1. Select **Cognito User Pool** as one of the **Enabled Identity Providers**\.
**Note**  
To sign in with external identity providers \(IdPs\) such as Facebook, Amazon, Google, and Apple, as well as through OpenID Connect \(OIDC\) or SAML IdPs, first configure them as described next, and then return to the **App client settings** page to enable them\.

1. Enter a callback URL for the Amazon Cognito authorization server to call after users are authenticated\. For a web app, the URL should start with `https://`, such as `https://www.example.com`\. 

   For an iOS or Android app, you can use a callback URL such as `myapp://`\.

1. Enter a Sign out URL\.

1. Select **Authorization code grant** to return an authorization code that is then exchanged for user pool tokens\. Because the tokens are never exposed directly to an end user, they are less likely to become compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\. For security reasons, we recommend that you use the authorization code grant flow, together with [Proof Key for Code Exchange \(PKCE\)](https://tools.ietf.org/html/rfc7636), for mobile apps\. 

1. Under **Allowed OAuth Flows**, select **Implicit grant** to have user pool JSON web tokens \(JWT\) returned to you from Amazon Cognito\. You can use this flow when there's no backend available to exchange an authorization code for tokens\. It's also helpful for debugging tokens\.
**Note**  
You can enable both the **Authorization code grant** and the **Implicit code grant**, and then use each grant as needed\. 

1. Unless you specifically want to exclude one, select the check boxes for all of the **Allowed OAuth scopes**\.
**Note**  
Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.

1. Choose **Save changes**\.

1. On the **Domain name** page, type a domain prefix that's available\.

1. Make a note of the complete domain address\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To create an app in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\. If you create a new user pool, you will be prompted to set up an app client and configure the hosted UI during the wizard\.

1. Choose the **App integration** tab for your user pool\.

1. Next to **Domain**, choose **Actions**, and then select either **Create custom domain** or **Create Cognito domain**\. If you have already configured a user pool domain, choose **Delete Cognito domain** or **Delete custom domain** before creating your new custom domain\.

1. Enter an available domain prefix to use with a **Cognito domain**\. For information on setting up a **Custom domain**, see [Using Your Own Domain for the Hosted UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-add-custom-domain.html)

1. Choose **Create**\.

1. Navigate back to the **App integration** tab for the same user pool and locate **App clients**\. Choose **Create an app client**\.

1. Choose an **Application type**\. Some recommended settings will be provided based on your selection\. An app that uses the hosted UI is a **Public client**\.

1. Enter an **App client name**\.

1. For this exercise, choose **Don't generate client secret**\. The client secret is used by confidential apps that authenticate users from a centralized application\. In this exercise, you will present a hosted UI sign\-in page to your users and will not require a client secret\.

1. Choose the **Authentication flows** you will allow with your app\. Ensure that `USER_SRP_AUTH` has been selected\.

1. Customize **token expiration**, **Advanced security configuration**, and **Attribute read and write permissions** as needed\. For more information, see [Configuring App Client Settings](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-settings.html)\.

1. Choose **Create**\.

1. In the **App client** screen, select **Edit** next to **Hosted UI settings**\.

1. **Add a callback URL** for your app client\. This is where you will be directed after hosted UI authentication\. You do not need to add an **Allowed sign\-out URL** until you are able to implement sign\-out in your app\.

   For an iOS or Android app, you can use a callback URL such as `myapp://`\.

1. Select the **Identity providers** for the app client\. At minimum, enable **Cognito user pool** as a provider\.
**Note**  
To sign in with external identity providers \(IdPs\) such as Facebook, Amazon, Google, and Apple, as well as through OpenID Connect \(OIDC\) or SAML IdPs, first configure them as shown in [Add Social Sign\-in to a User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-social-idp.html), and then return to the **App client settings** page to enable them\.

1. Choose **OAuth 2\.0 Grant Types**\. Select **Authorization code grant** to return an authorization code that is then exchanged for user pool tokens\. Because the tokens are never exposed directly to an end user, they are less likely to become compromised\. However, a custom application is required on the backend to exchange the authorization code for user pool tokens\. For security reasons, we recommend that you use the authorization code grant flow, together with [Proof Key for Code Exchange \(PKCE\)](https://tools.ietf.org/html/rfc7636), for mobile apps\. 

   Select **Implicit grant** to have user pool JSON web tokens \(JWT\) returned to you from Amazon Cognito\. You can use this flow when there's no backend available to exchange an authorization code for tokens\. It's also helpful for debugging tokens\.
**Note**  
You can enable both the **Authorization code grant** and the **Implicit code grant**, and then use each grant as needed\.   
Select **Client credentials** only if your app needs to request access tokens on its own behalf, not on behalf of a user\.

1. Unless you specifically want to exclude one, select all **OpenID Connect scopes**\.

1. Select any **Custom scopes** you have configured\. Custom scopes are typically used with confidential clients\.

1. Choose **Save changes**\.

1. On the **Domain name** page, type a domain prefix that's available\.

1. Make a note of the complete domain address\.

1. Choose **Save changes**\.

------

**To view your sign\-in page**

From your **App client page**, select **View hosted UI** to open a new browser tab to a sign\-in page pre\-populated with app client id, scope, grant, and callback URL parameters\.

You can view the hosted UI sign\-in webpage manually with the following URL\. Note the `response_type`\. In this case, **response\_type=code** for the authorization code grant\.

```
https://your_domain/login?response_type=code&client_id=your_app_client_id&redirect_uri=your_callback_url
```

You can view the hosted UI sign\-in webpage with the following URL for the implicit code grant where **response\_type=token**\. After a successful sign\-in, Amazon Cognito returns user pool tokens to your web browser's address bar\.

```
https://your_domain/login?response_type=token&client_id=your_app_client_id&redirect_uri=your_callback_url
```

You can find the JSON web token \(JWT\) identity token after the `#idtoken=` parameter in the response\.

The following URL is a sample response from an implicit grant request\. Your identity token string will be much longer\.

```
https://www.example.com/#id_token=123456789tokens123456789&expires_in=3600&token_type=Bearer  
```

Amazon Cognito user pools tokens are signed using an RS256 algorithm\. You can decode and verify user pool tokens using AWS Lambda, see [Decode and verify Amazon Cognito JWT tokens](https://github.com/awslabs/aws-support-tools/tree/master/Cognito/decode-verify-jwt) on the AWS GitHub website\.

Your domain is shown on the **Domain name** page\. Your app client ID and callback URL are shown on the **General settings** page\. If the changes you made in the console do not appear immediately, wait a few minutes and then refresh your browser\.

## Next step<a name="cognito-user-pools-configuring-app-integration-next-step"></a>

[Step 3\. Add social sign\-in to a user pool \(optional\)](cognito-user-pools-configuring-federation-with-social-idp.md)