# Using Amazon Pinpoint Analytics with Amazon Cognito User Pools<a name="cognito-user-pools-pinpoint-integration"></a>

Amazon Cognito User Pools are integrated with Amazon Pinpoint to provide analytics for Amazon Cognito user pools and to enrich the user data for Amazon Pinpoint campaigns\. Amazon Pinpoint provides analytics and targeted campaigns to drive user engagement in mobile apps using push notifications\. With Amazon Pinpoint analytics support in Amazon Cognito user pools, you can track user pool sign\-ups, sign\-ins, failed authentications, daily active users \(DAUs\), and monthly active users \(MAUs\) in the Amazon Pinpoint console\. You can drill into the data for different date ranges or attributes, such as device platform, device locale, and app version\.

You can also set up user attributes that are specific to your app using the AWS Mobile SDK for Android or AWS Mobile SDK for iOS\. Those can then be used to segment your users on Amazon Pinpoint and send them targeted push notifications\. If you choose **Share user attribute data with Amazon Pinpoint** in the **Analytics** tab in the Amazon Cognito console, additional endpoints are created for user email addresses and phone numbers\.

## Find Amazon Cognito and Amazon Pinpoint Region mappings<a name="cognito-user-pools-find-region-mappings"></a>

The following table shows Region mappings between Amazon Cognito and Amazon Pinpoint\. Use the table to find the Region where you built your Amazon Cognito user pool and the corresponding Amazon Pinpoint Region\. Next, use these Regions to integrate Amazon Cognito and your Amazon Pinpoint project\.


| Amazon Cognito regions that support Amazon Pinpoint | Amazon Pinpoint project regions | 
| --- | --- | 
|  ap\-northeast\-1 ap\-northeast\-2 ap\-south\-1 ap\-southeast\-1 ap\-southeast\-2 ca\-central\-1 eu\-central\-1 eu\-west\-1 eu\-west\-2 us\-east\-1 us\-east\-2 us\-west\-2  |  us\-east\-1 us\-east\-1 us\-east\-1, ap\-south\-1 us\-east\-1 us\-east\-1, ap\-southeast\-2 us\-east\-1 us\-east\-1, eu\-central\-1 us\-east\-1, eu\-west\-1 us\-east\-1 us\-east\-1 us\-east\-1 us\-east\-1, us\-west\-2  | 

**Region mapping examples**
+ If you create a user pool in ap\-northest\-1, you have to create your Amazon Pinpoint project in us\-east\-1\.
+ If you create a user pool in ap\-south\-1, you have to create your Amazon Pinpoint project in either us\-east\-1 or ap\-south\-1\.

**Note**  
Amazon Pinpoint is available in several AWS Regions in North America, Europe, Asia, and Oceania\. Besides the exceptions in the table, Amazon Cognito will only support in\-Region Amazon Pinpoint integrations\. If Amazon Pinpoint is available the same Region as Amazon Cognito, then Amazon Cognito sends events to Amazon Pinpoint projects within the same Region\. If Amazon Pinpoint isn't available in the Region, then Amazon Cognito doesn't support Amazon Pinpoint integrations in that Region until Amazon Pinpoint becomes available\. For Amazon Pinpoint detailed Region information, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html)\.

### Specifying Amazon Pinpoint Analytics Settings \(AWS Management Console\)<a name="cognito-user-pools-pinpoint-integration-console"></a>

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
In Amazon Pinpoint, each project should be a single app\. For example, if a game developer has two games, each game should be a separate Amazon Pinpoint project, even if both games use the same Amazon Cognito user pool\. For more information on Amazon Pinpoint projects, see [Create a project in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/developerguide/mobile-push-create-project.html)\. 

1. Choose **Share user attribute data with Amazon Pinpoint** if you want Amazon Cognito to send email addresses and phone numbers to Amazon Pinpoint in order to create additional endpoints for users\. After the account phone number and email address are verified, they are only shared with Amazon Pinpoint if they are available to the user account\.
**Note**  
An *endpoint* uniquely identifies a user device to which you can send push notifications with Amazon Pinpoint\. For more information about endpoints, see [Adding Endpoints](https://docs.aws.amazon.com/pinpoint/latest/developerguide/endpoints.html) in the *Amazon Pinpoint Developer Guide*\.

1. Choose **Save changes**\.

1. To specify additional app mappings, choose **Add another app mapping**\.

1. Choose **Save changes**\.

### Specifying Amazon Pinpoint Analytics Settings \(AWS CLI and AWS API\)<a name="cognito-user-pools-pinpoint-integration-cli-api"></a>

Use the following commands to specify Amazon Pinpoint analytics settings for your user pool\.

**To specify the analytics settings for your user pool's existing client app at app creation time**
+ AWS CLI: `aws cognito-idp create-user-pool-client`
+ AWS API: [CreateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html)

**To update the analytics settings for your user pool's existing client app**
+ AWS CLI: `aws cognito-idp update-user-pool-client`
+ AWS API: [UpdateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html)

**Note**  
Amazon Cognito supports in\-Region integrations when you use `ApplicationArn`