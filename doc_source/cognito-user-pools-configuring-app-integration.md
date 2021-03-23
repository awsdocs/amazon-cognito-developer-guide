# Step 2\. Add an App to Enable the Hosted Web UI<a name="cognito-user-pools-configuring-app-integration"></a>

After you create a user pool, you can create an app to use the built\-in webpages for signing up and signing in your users\.

**To create an app in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the navigation bar on the left\-side of the page, choose **App clients** under **General settings**\.

1. Choose **Add an app client**\.

1. Give your app a name\.

1. Clear the option **Generate client secret** for the purposes of this getting started exercise, as it would not be secure to send it on the URL using client\-side JavaScript\. The client secret is used by applications that have a server\-side component that can secure the client secret\. 

1. Choose **Create app client**\.

1. Note the **App client ID**\.

1. Choose **Return to pool details**\.

1. Choose **App client settings** from the navigation bar on the left\-side of the console page\.

1. Select **Cognito User Pool** as one of the **Enabled Identity Providers**\.
**Note**  
To sign in with external identity providers \(IdPs\) such as Facebook, Amazon, Google, and Apple, as well as through OpenID Connect \(OIDC\) or SAML IdPs, first configure them as described next, and then return to the **App client settings** page to enable them\.

1. Enter a callback URL for the Amazon Cognito authorization server to call after users are authenticated\. For a web app, the URL should start with `https://`, such as https://www\.example\.com\. 

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

**To view your sign\-in page**

You can view the hosted UI sign\-in webpage with the following URL\. Note the `response_type`\. In this case, **response\_type=code** for the authorization code grant\.

```
https://your_domain/login?response_type=code&client_id=your_app_client_id&redirect_uri=your_callback_url
```

You can view the hosted UI sign\-in webpage with the following URL for the implicit code grant where **response\_type=token**\. After a successful sign\-in, Amazon Cognito returns user pool tokens to your web browser's address bar\.

```
https://your_domain/login?response_type=token&client_id=your_app_client_id&redirect_uri=your_callback_url
```

You can find the JSON web token \(JWT\) identity token after the `#idtoken=` parameter in the response\.

Here's a sample response from an implicit grant request\. Your identity token string will be much longer\.

```
https://www.example.com/#id_token=123456789tokens123456789&expires_in=3600&token_type=Bearer  
```

You can decode and verify user pool tokens using AWS Lambda, see [Decode and verify Amazon Cognito JWT tokens](https://github.com/awslabs/aws-support-tools/tree/master/Cognito/decode-verify-jwt) on the AWS GitHub website\.

Amazon Cognito user pools tokens are signed using an RS256 algorithm\.

You might have to wait a minute to refresh your browser before changes you made in the console appear\.

Your domain is shown on the **Domain name** page\. Your app client ID and callback URL are shown on the **General settings** page\.

## Next Step<a name="cognito-user-pools-configuring-app-integration-next-step"></a>

[Step 3\. Add Social Sign\-in to a User Pool \(Optional\)](cognito-user-pools-configuring-federation-with-social-idp.md)