# SMS text message MFA<a name="user-pool-settings-mfa-sms-text-message"></a>

When a user signs in with MFA enabled, they first enter and submit their user name and password\. The client app receives a `getMFA` response that indicates where the authorization code was sent\. The client app should indicate to the user where to look for the code \(such as which phone number the code was sent to\)\. Next, it provides a form for entering the code\. Finally, the client app submits the code to complete the sign\-in process\. The destination is masked, which hides all but the last four digits of the phone number\. If an app is using the Amazon Cognito hosted UI, it shows a page for the user to enter the MFA code\.

The SMS text message authorization code is valid for 3 minutes\.

If a user no longer has access to their device where the SMS text message MFA codes are sent, they must request help from your customer service office\. An administrator with necessary AWS account permissions can change the user's phone number, but only through the AWS CLI or the API\.

When a user successfully goes through the SMS text message MFA flow, their phone number is also marked as verified\.

**Note**  
SMS for MFA is charged separately\. \(There is no charge for sending verification codes to email addresses\.\) For information about Amazon SNS pricing, see [Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\. For the current list of countries where SMS messaging is available, see [Supported Regions and Countries](https://docs.aws.amazon.com/sns/latest/dg/sms_supported-countries.html)\. 

**Important**  
To ensure that SMS messages are sent to verify phone numbers and for SMS text message MFA, you must request an increased spend limit from Amazon SNS\.  
Amazon Cognito uses Amazon SNS for sending SMS messages to users\. The number of SMS messages Amazon SNS delivers is subject to spend limits\. Spend limits can be specified for an AWS account and for individual messages, and the limits apply only to the cost of sending SMS messages\.  
The default spend limit per account \(if not specified\) is 1\.00 USD per month\. If you want to raise the limit, submit an [SNS Limit Increase case](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the AWS Support Center\. For **New limit value**, enter your desired monthly spend limit\. In the **Use Case Description** field, explain that you're requesting an SMS monthly spend limit increase\.

To add MFA to your user pool, see [Adding MFA to a user pool](user-pool-settings-mfa.md)\.