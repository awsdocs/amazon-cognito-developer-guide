# Using Amazon Pinpoint analytics with Amazon Cognito user pools<a name="cognito-user-pools-pinpoint-integration"></a>

Amazon Cognito user pools are integrated with Amazon Pinpoint to provide analytics for Amazon Cognito user pools and to enrich the user data for Amazon Pinpoint campaigns\. Amazon Pinpoint provides analytics and targeted campaigns to drive user engagement in mobile apps using push notifications\. With Amazon Pinpoint analytics support in Amazon Cognito user pools, you can track user pool sign\-ups, sign\-ins, failed authentications, daily active users \(DAUs\), and monthly active users \(MAUs\) in the Amazon Pinpoint console\. You can drill into the data for different date ranges or attributes, such as device platform, device locale, and app version\.

You can also set up custom attributes for your app\. Those can then be used to segment your users on Amazon Pinpoint and send them targeted push notifications\. If you choose **Share user attribute data with Amazon Pinpoint** in the **Analytics** tab in the Amazon Cognito console, Amazon Pinpoint creates additional endpoints for user email addresses and phone numbers\.

## Amazon Cognito and Amazon Pinpoint Region availability<a name="cognito-user-pools-find-region-mappings"></a>

The following table shows the AWS Region mappings between Amazon Cognito and Amazon Pinpoint that meet one of the following conditions\.
+ You can only use an Amazon Pinpoint project in the US East \(N\. Virginia\) \(us\-east\-1\) Region\.
+ You can use an Amazon Pinpoint project in the same Region *or* in the US East \(N\. Virginia\) \(us\-east\-1\) Region

By default, Amazon Cognito can only send analytics to a Amazon Pinpoint project in the same AWS Region\. The exceptions to this rule are the Regions in the following table, and Regions where Amazon Pinpoint in unavailable\.

**Note**  
Amazon Pinpoint isn't available in Europe \(Milan\) and Middle East \(Bahrain\)\. Amazon Cognito user pools in these Regions don't support analytics\.

The table shows the relation between the Region where you built your Amazon Cognito user pool and the corresponding Region in Amazon Pinpoint\. You must configure your Amazon Pinpoint project in an available Region to integrate it with Amazon Cognito\.


| Amazon Cognito user pool Region | Region for Amazon Pinpoint project | 
| --- | --- | 
|  ap\-northeast\-1  |  us\-east\-1  | 
|  ap\-northeast\-2  |  us\-east\-1  | 
|  ap\-south\-1  |  us\-east\-1, ap\-south\-1  | 
|  ap\-southeast\-1  |  us\-east\-1  | 
|  ap\-southeast\-2  |  us\-east\-1, ap\-southeast\-2  | 
|  ca\-central\-1  |  us\-east\-1  | 
|  eu\-central\-1  |  us\-east\-1, eu\-central\-1  | 
|  eu\-west\-1  |  us\-east\-1, eu\-west\-1  | 
|  eu\-west\-2  |  us\-east\-1  | 
|  us\-east\-1  |  us\-east\-1  | 
|  us\-east\-2  |  us\-east\-1  | 
|  us\-west\-2  |  us\-east\-1, us\-west\-2  | 

**Region mapping examples**
+ If you create a user pool in ap\-northeast\-1, you can create your Amazon Pinpoint project in us\-east\-1\.
+ If you create a user pool in ap\-south\-1, you can create your Amazon Pinpoint project in either us\-east\-1 or ap\-south\-1\.

**Note**  
For all AWS Regions except those in the preceding table, Amazon Cognito can only use an Amazon Pinpoint project in the same Region as your user pool\. If Amazon Pinpoint isn't available in the Region where you built your user pool, and it's not listed in the table, then Amazon Cognito doesn't support Amazon Pinpoint analytics in that Region\. For detailed AWS Region information, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html)\.

### Specifying Amazon Pinpoint analytics settings \(AWS Management Console\)<a name="cognito-user-pools-pinpoint-integration-console"></a>

You can configure your Amazon Cognito user pool to send analytics data to Amazon Pinpoint\. Amazon Cognito only sends analytics data to Amazon Pinpoint for users that are native to your user pool\. After you configure your user pool to associate with a Amazon Pinpoint project, you must include `AnalyticsMetadata` in your API requests\. For more information, see [Integrating your app with Amazon Pinpoint](#cognito-user-pools-pinpoint-integration-client)\.

------
#### [ Original console ]

**To specify analytics settings**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, **Manage User Pools**, and choose the user pool you want to edit\.

1. Choose the **Analytics** tab\.

1. Choose **Add analytics and campaigns**\.

1. Choose a **Cognito app client** from the list\.

1. To map your Amazon Cognito app to an **Amazon Pinpoint project**, choose the Amazon Pinpoint project from the list\.
**Note**  
The Amazon Pinpoint project ID is a 32\-character string that is unique to your Amazon Pinpoint project\. It is listed in the Amazon Pinpoint console\.  
You can map multiple Amazon Cognito apps to a single Amazon Pinpoint project\. However, each Amazon Cognito app can only be mapped to one Amazon Pinpoint project\.  
In Amazon Pinpoint, each project should be a single app\. For example, if a game developer has two games, each game should be a separate Amazon Pinpoint project, even if both games use the same Amazon Cognito user pool\. For more information about Amazon Pinpoint projects, see [Create a project in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/developerguide/mobile-push-create-project.html)\. 

1. Choose **Share user attribute data with Amazon Pinpoint** if you want Amazon Cognito to send email addresses and phone numbers to Amazon Pinpoint in order to create additional endpoints for users\. After the account phone number and email address are verified, they are only shared with Amazon Pinpoint if they are available to the user account\.
**Note**  
An *endpoint* uniquely identifies a user device to which you can send push notifications with Amazon Pinpoint\. For more information about endpoints, see [Adding endpoints](https://docs.aws.amazon.com/pinpoint/latest/developerguide/endpoints.html) in the *Amazon Pinpoint Developer Guide*\.

1. Choose **Save changes**\.

1. To specify additional app mappings, choose **Add another app mapping**\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To specify analytics settings**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Select **User Pools** and choose an existing user pool from the list\.

1. Choose the **App integration** tab\.

1. Under **App clients and analytics**, choose an existing **App client name** from the list\.

1. Under **Pinpoint analytics**, choose **Enable**\.

1. Choose a **Pinpoint Region**\.

1. Choose an **Amazon Pinpoint project** or select **Create Amazon Pinpoint project**\.
**Note**  
The Amazon Pinpoint project ID is a 32\-character string that is unique to your Amazon Pinpoint project\. It is listed in the Amazon Pinpoint console\.  
You can map multiple Amazon Cognito apps to a single Amazon Pinpoint project\. However, each Amazon Cognito app can only be mapped to one Amazon Pinpoint project\.  
In Amazon Pinpoint, each project should be a single app\. For example, if a game developer has two games, each game should be a separate Amazon Pinpoint project, even if both games use the same Amazon Cognito user pool\. For more information on Amazon Pinpoint projects, see [Create a project in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/developerguide/mobile-push-create-project.html)\. 

1. Under **User data sharing**, choose **Share user data with Amazon Pinpoint** if you want Amazon Cognito to send email addresses and phone numbers to Amazon Pinpoint and create additional endpoints for users\. After your users verify their email address and phone number, Amazon Cognito only shares them with Amazon Pinpoint if they are available to the user account\.
**Note**  
An *endpoint* uniquely identifies a user device to which you can send push notifications with Amazon Pinpoint\. For more information about endpoints, see [Adding endpoints](https://docs.aws.amazon.com/pinpoint/latest/developerguide/endpoints.html) in the *Amazon Pinpoint Developer Guide*\.

1. Choose **Save changes**\.

------

### Specifying Amazon Pinpoint analytics settings \(AWS CLI and AWS API\)<a name="cognito-user-pools-pinpoint-integration-cli-api"></a>

Use the following commands to specify Amazon Pinpoint analytics settings for your user pool\.

**To specify the analytics settings for your user pool's existing client app at app creation time**
+ AWS CLI: `aws cognito-idp create-user-pool-client`
+ AWS API: [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html)

**To update the analytics settings for your user pool's existing client app**
+ AWS CLI: `aws cognito-idp update-user-pool-client`
+ AWS API: [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html)

**Note**  
Amazon Cognito supports in\-Region integrations when you use `ApplicationArn`

## Integrating your app with Amazon Pinpoint<a name="cognito-user-pools-pinpoint-integration-client"></a>

You can publish analytics metadata to Amazon Pinpoint for Amazon Cognito *native users* in the *native API*\.

**Native users**  
Users who signed up for an account or were created in your user pool instead of signing in through a third\-party identity provider \(IdP\)\.

**Native API**  
The operations that you can integrate with an AWS SDK, using an app with a custom user interface \(UI\)\. You can't pass analytics metadata for federated or native users who sign in through the hosted UI\. See the [Amazon Cognito API Reference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html) for a list of native API operations\.

After you configure your user pool to publish to a campaign, Amazon Cognito passes metadata to Amazon Pinpoint for the following API operations\.
+ `AdminInitiateAuth`
+ `AdminRespondToAuthChallenge`
+ `ConfirmForgotPassword`
+ `ConfirmSignUp`
+ `ForgotPassword`
+ `InitiateAuth`
+ `ResendConfirmationCode`
+ `RespondToAuthChallenge`
+ `SignUp`

To pass metadata about your user's session to your Amazon Pinpoint campaign, include an `AnalyticsEndpointId` value in the `AnalyticsMetadata` parameter of your API request\. For a JavaScript example, see [Why aren't my Amazon Cognito user pool analytics appearing on my Amazon Pinpoint dashboard?](http://aws.amazon.com/premiumsupport/knowledge-center/pinpoint-cognito-user-pool-analytics/) in the *AWS Knowledge Center*\.