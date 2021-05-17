# Configuring Email or Phone Verification<a name="user-pool-settings-email-phone-verification"></a>

You can choose settings for email or phone verification in the **MFA and verifications** tab\. For more information on MFA,see [SMS Text Message MFA](user-pool-settings-mfa-sms-text-message.md)

Amazon Cognito can automatically verify email addresses or mobile phone numbers by sending a verification code—or, for email, a verification link\. For email addresses, the code or link is sent in an email message\. For phone numbers, the code is sent in an SMS text message\.

Verification of a phone or email is necessary to automatically confirm users and enable recovery from forgotten passwords\. Alternatively, you can automatically confirm users with the pre\-sign up Lambda trigger or by using the [AdminConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/AdminConfirmSignUp.html) API\. For more information, see [Signing Up and Confirming User Accounts](signing-up-users-in-your-app.md)\.

The verification code or link is valid for 24 hours\.

If verification is selected as required for email or phone, the verification code or link is automatically sent when a user signs up\.

**Notes**  
Use of SMS text messaging for verifying phone numbers is charged separately by Amazon SNS\. \(There is no charge for sending verification codes to email addresses\.\) For information about Amazon SNS pricing, see [Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\. For the current list of countries where SMS messaging is available, see [Supported Regions and Countries](https://docs.aws.amazon.com/sns/latest/dg/sms_supported-countries.html)\. 
When you test actions in your app that initiate emails from Amazon Cognito, use a real email address that Amazon Cognito can send to without incurring hard bounces\. For more information, see [Sending Emails While Testing Your App](signing-up-users-in-your-app.md#managing-users-accounts-email-testing)\.
The forgotten password flow requires either the user's email or the user's phone number to be verified\.

**Important**  
If a user signs up with both a phone number and an email address, and your user pool settings require verification of both attributes, a verification code is sent via SMS to the phone\. The email address is not verified, so your app needs to call [GetUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html) to see if an email address is awaiting verification\. If it is, the app should call [GetUserAttributeVerificationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUserAttributeVerificationCode.html) to initiate the email verification flow and then submit the verification code by calling [VerifyUserAttribute](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifyUserAttribute.html)\.

Spend limits can be specified for an AWS account and for individual messages, and the limits apply only to the cost of sending SMS messages\. For more information, see [Amazon SNS FAQs](http://aws.amazon.com/sns/faqs/)\.

SMS messages from Amazon Cognito user pools are routed through Amazon SNS in the same region unless noted in the following table\.


| Amazon Cognito Region | Supported SNS Regions | 
| --- | --- | 
| US East \(Ohio\) us\-east\-2 | us\-east\-1 | 
| Asia Pacific \(Mumbai\) ap\-south\-1 | ap\-southeast\-1 | 
| Asia Pacific \(Seoul\) ap\-northeast\-2 | ap\-northeast\-1 | 
| Canada\(Central\) ca\-central\-1 | us\-east\-1 | 
| Europe \(Frankfurt\) eu\-central\-1 | eu\-west\-1 | 
| Europe \(London\) eu\-west\-2 | eu\-west\-1 | 

**Example: **If your Cognito user pool is in us\-east\-1 region, you can update the Amazon SNS limit in us\-east\-1 region\. 

**Example: **If your Cognito user pool is in ap\-south\-1 region, you can update the Amazon SNS limit in ap\-southeast\-1 region\.

## Authorizing Amazon Cognito to Send SMS Messages on Your Behalf<a name="user-pool-settings-verifications-iam-role-for-sms"></a>

To send SMS messages to your users on your behalf, Amazon Cognito needs your permission\. To grant that permission, you can create an AWS Identity and Access Management \(IAM\) role in the **MFA and verifications** tab of the Amazon Cognito console by choosing **Create role**\.
