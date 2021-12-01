# To configure an app client \(AWS Management Console\)<a name="cognito-user-pools-app-idp-settings-console"></a>

------
#### [ Original console ]

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the navigation bar on the left\-side of the page, choose **App clients** under **General settings**\.

1. Choose **Add an app client**\.

1. Enter your app name\.

1. Unless required by your authorization flow, clear the option **Generate client secret**\. The client secret is used by applications that have a server\-side component that can secure the client secret\.

1. \(Optional\) Change the token expiration settings\.

1. Select **Auth Flows Configuration** options\. For more information, see [User Pool Authentication Flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html)\.

1. Choose a **Security configuration**\. We recommend you select **Enabled**\.

1. \(Optional\) Choose **Set attribute read and write permissions**\.

1. Choose **Create app client**\.

1. Note the **App client id**\.

1. Choose **Return to pool details**\.

------
#### [ New console ]

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **App integration** tab\.

1. Under **App clients**, select **Create an app client**\.

1. Select an **App type**: **Public client**, **Confidential client**, or **Other**\. A **Public client** typically operates from your users' devices and uses unauthenticated and token\-authenticated APIs\. A **Confidential client** typically operates from an app on a central server that you trust with client secrets and API credentials, and uses authorization headers and AWS Identity and Access Management credentials to sign requests\. If your use case is different from the preconfigured app client settings for a **Public client** or a **Confidential client**, select **Other**\.

1. Enter an **App client name**\.

1. Select the **Authentication flows** you want to allow in your app client\. For more information, see [User Pool Authentication Flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html)\.

1. \(Optional\) configure token expiration\.

   1. Specify the **Refresh token expiration** for the app client\. The default value is 30 days\. You can change it to any value between 1 hour and 10 years\.

   1. Specify the **Access token expiration** for the app client\. The default value is 1 hour\. You can change it to any value between 5 minutes and 24 hours\.

   1. Specify the **ID token expiration** for the app client\. The default value is 1 hour\. You can change it to any value between 5 minutes and 24 hours\.
**Important**  
If you use the hosted UI and configure a token lifetime of less than an hour, your user will be able to use tokens based on their session cookie duration, which is currently fixed at one hour\.

1. Choose **Generate client secret** to have Amazon Cognito generate a client secret for you\. Client secrets are typically associated with confidential clients\.

1. Choose whether you will **Enable token revocation** for this app client\. This will increase the size of tokens that Amazon Cognito issues\. For more information, see [Revoking Tokens](https://docs.aws.amazon.com/cognito/latest/developerguide/token-revocation.html)\.

1. Choose whether you will **Prevent error messages that reveal user existence** for this app client\. Amazon Cognito will respond to sign\-in requests for nonexistent users with a generic message stating that either the user name or password was incorrect\.

1. \(Optional\) Configure **Attribute read and write permissions** for this app client\. Your app client can have permission to read and write only a limited subset of your user pool's attribute schema\. For more information, see [Attribute Permissions and Scopes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-attribute-permissions-and-scopes)\.

1. Choose **Create**\.

1. Note the **Client id**\. This will identify the app client in sign\-up and sign\-in requests\.

------