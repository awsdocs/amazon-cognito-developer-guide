# Configuring user pool analytics<a name="user-pool-settings-analytics"></a>

**Note**  
The **Analytics** tab appears only when you're editing an existing user pool\.

Using Amazon Pinpoint Analytics, you can track Amazon Cognito user pools sign\-ups, sign\-ins, failed authentications, daily active users \(DAUs\), and monthly active users \(MAUs\)\. You can also set up user attributes specific to your app using the AWS Mobile SDK for Android or AWS Mobile SDK for iOS\. Those can then be used to segment your users in Amazon Pinpoint and send them targeted push notifications\.

In the **Analytics** tab, you can specify an Amazon Pinpoint project for your Amazon Cognito app client\. For more information, see [Using Amazon Pinpoint Analytics with Amazon Cognito user pools](cognito-user-pools-pinpoint-integration.md)\.

**Note**  
 Amazon Pinpoint is available in several AWS Regions in North America, Europe, Asia, and Oceania\. Amazon Pinpoint Regions include the Amazon Pinpoint API\. If an Amazon Pinpoint region is supported by Amazon Cognito, then Amazon Cognito will send events to Amazon Pinpoint projects within the *same* Amazon Pinpoint region\. If a region *isn't* supported by Amazon Pinpoint, then Amazon Cognito will *only* support sending events in us\-east\-1\. For Amazon Pinpoint detailed region information, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html) and [Using Amazon Pinpoint analytics with Amazon Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-pinpoint-integration.html)\. 

**To add analytics and campaigns**

1. Choose **Add analytics and campaigns**\.

1. Choose a **Cognito app client** from the list\.

1. To map your Amazon Cognito app to an **Amazon Pinpoint project**, choose the Amazon Pinpoint project from the list\.
**Note**  
The Amazon Pinpoint project ID is a 32\-character string that is unique to your Amazon Pinpoint project\. It is listed the Amazon Pinpoint console\.  
You can map multiple Amazon Cognito apps to a single Amazon Pinpoint project\. However, each Amazon Cognito app can only be mapped to one Amazon Pinpoint project\.  
In Amazon Pinpoint, each project should be a single app\. For example, if a game developer has two games, each game should be a separate Amazon Pinpoint project, even if both games use the same Amazon Cognito user pools\.

1. Choose **Share user attribute data with Amazon Pinpoint** if you want Amazon Cognito to send email addresses and phone numbers to Amazon Pinpoint in order to create additional endpoints for users\.
**Note**  
An *endpoint* uniquely identifies a user device where you can send push notifications with Amazon Pinpoint\. For more information about endpoints, see [Adding endpoints to Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/developerguide/audience-define-endpoints.html) in the *Amazon Pinpoint Developer Guide*\.

1. Enter an **IAM role** that you already created or choose **Create new role** to create a new role in the IAM console\.

1. Choose **Save changes**\.

1. To specify additional app mappings, choose **Add app mapping**\.

1. Choose **Save changes**\.