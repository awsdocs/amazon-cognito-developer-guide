# Configuring a user pool app client<a name="user-pool-settings-client-apps"></a>

An app is an entity within a user pool that has permission to call unauthenticated API operations\. Unauthenticated API operations are those that do not have an authenticated user\. Examples include operations to register, sign in, and handle forgotten passwords\. To call these API operations, you need an app client ID and an optional client secret\. It is your responsibility to secure any app client IDs or secrets so that only authorized client apps can call these unauthenticated operations\.

You can create multiple apps for a user pool\. Typically, an app corresponds to the platform of an app\. For example, you might create an app for a server\-side application and a different Android app\. Each app has its own app client ID\.

When you create an app client in Amazon Cognito, you can pre\-populate options based on the standard OAuth client types **public client** and **confidential client**\. Configure a **confidential client** with a **client secret**\.

**Public client**  
A public client runs in a browser or on a mobile device\. Because it does not have trusted server\-side resources, it does not have a client secret\.

**Confidential client**  
A confidential client has server\-side resources that can be trusted with a **client secret** for unauthenticated API operations\. The app might run as a daemon or shell script on your backend server\.

**Client secret**  
A client secret is a fixed string that your app must use in all API requests to the app client\. Your app client must have a client secret to perform `client_credentials` grants\.  
You can't change secrets after you create an app\. You can create a new app with a new secret if you want to rotate the secret\. You can also delete an app to block access from apps that use that app client ID\.

You can use a confidential client, and a client secret, with a public app\. Use an Amazon CloudFront proxy to add a `SECRET_HASH` in transit\. For more information, see [Protect public clients for Amazon Cognito by using an Amazon CloudFront proxy](https://aws.amazon.com/blogs/security/protect-public-clients-for-amazon-cognito-by-using-an-amazon-cloudfront-proxy/) on the AWS blog\.

**Creating an app client \(AWS Management Console\)**

------
#### [ Original console ]

**To create an app client \(console\)**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **App integration** tab\. Locate **App clients** and select **Create an app client**\.

1. Choose **Add app client**\.

1. Choose **Add an app client**\.

1. Enter an **App client name**\.

1. Specify the **Refresh token expiration** for the app\. The default value is 30 days\. You can change this default to any value between 1 hour and 10 years\.

1. Specify the app's **Access token expiration**\. The default value is 1 hour\. You can change this default to any value between 5 minutes and 24 hours\.

1. Specify the **ID token expiration** for the app\. The default value is 1 hour\. You can change this default to any value between 5 minutes and 24 hours\.
**Important**  
If you use the hosted UI and set token expiration to less than an hour, your user can get new tokens based on their session cookie, which expires after 1 hour\. You can't change the duration of this cookie\.

1. By default, user pools generate a client secret for your app\. If you do not need a client secret, clear **Generate client secret**\.

1. If your server app requires developer credentials \(using [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\) and doesn't use [Secure remote password \(SRP\) authentication](http://srp.stanford.edu/), select **Enable username password auth for admin APIs for authentication \(ALLOW\_ADMIN\_USER\_PASSWORD\_AUTH\)** to activate server\-side authentication\. For more information, see [Admin authentication flow](amazon-cognito-user-pools-authentication-flow.md#amazon-cognito-user-pools-admin-authentication-flow)\. 

1. Under **Prevent User Existence Errors**, choose **Legacy** or **Enabled**\. For more information, see [Managing error response](https://docs.aws.amazon.com//cognito/latest/developerguide/cognito-user-pool-managing-errors.html)\. 

1. By default, user pools give your app permission to read and write all attributes\. If you want to set different permissions for your app, complete the following steps\. Otherwise, skip to Step 15\.

   1. Choose **Set attribute read and write permissions**\.

   1. Do either of the following to set read and write permissions: 
      + Choose one or more scopes\. Each scope is a set of standard attributes\. For more information, see the list of [standard OIDC scopes](http://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)\.
      + Choose individual standard or custom attributes\.
**Note**  
You can't remove required attributes from write permissions in any app\.

1. Choose **Create app client**\.

1. If you want to create another app, choose **Add an app**\.

1. Once you've created all the apps you want, choose **Return to pool details**, update any other fields, and then choose **Create pool**\.

------
#### [ New console ]

**To create an app client \(console\)**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Select the **App integration** tab\.

1. Under **App clients**, select **Create an app client**\.

1. Select an **App type**: **Public client**, **Confidential client**, or **Other**\.

1. Enter an **App client name**\.

1. Select the **Authentication flows** you want to allow in your app client\.

1. \(Optional\) If you want to configure token expiration, complete the following steps:

   1. Specify the **Refresh token expiration** for the app client\. The default value is 30 days\. You can change it to any value between 1 hour and 10 years\.

   1. Specify the **Access token expiration** for the app client\. The default value is 1 hour\. You can change it to any value between 5 minutes and 24 hours\.

   1. Specify the **ID token expiration** for the app client\. The default value is 1 hour\. You can change it to any value between 5 minutes and 24 hours\.
**Important**  
If you use the hosted UI and configure a token lifetime of less than an hour, your user will be able to use tokens based on their session cookie duration, which is currently fixed at one hour\.

1. Choose **Generate client secret** to have Amazon Cognito generate a client secret for you\. Client secrets are typically associated with confidential clients\.

1. Choose whether you will **Enable token revocation** for this app client\. This will increase the size of tokens that Amazon Cognito issues\.

1. Choose whether you will **Prevent error messages that reveal user existence** for this app client\. Amazon Cognito will respond to sign\-in requests for nonexistent users with a generic message stating that either the user name or password was incorrect\.

1. \(Optional\) Configure **Attribute read and write permissions** for this app client\. Your app client can have permission to read and write a limited subset of your user pool's attribute schema\.

1. Choose **Create**\.

1. Note the **Client id**\. This will identify the app client in sign\-up and sign\-in requests\.

------

**To create and update app clients in a user pool \(API, AWS CLI\)**  
Do one of the following: 
+ **API** – Use the [CreateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html) and [UpdateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html) operations\.
+ **AWS CLI** – At the command line, run the `create-user-pool-client` and `update-user-pool-client` commands\.