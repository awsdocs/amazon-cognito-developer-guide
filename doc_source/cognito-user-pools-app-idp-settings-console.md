# To Configure an App Client \(AWS Management Console\)<a name="cognito-user-pools-app-idp-settings-console"></a>

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