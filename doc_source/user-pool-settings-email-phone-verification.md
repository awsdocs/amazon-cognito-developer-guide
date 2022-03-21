# Configuring email or phone verification<a name="user-pool-settings-email-phone-verification"></a>

**Note**  
In the new Amazon Cognito console experience, you can manage verification in the **Sign\-up experience** tab of your user pool\.

You can choose settings for email or phone verification under the **MFA and verifications** tab\. For more information on multi\-factor authentication \(MFA\), see [SMS Text Message MFA](user-pool-settings-mfa-sms-text-message.md)\.

Amazon Cognito uses Amazon SNS to send SMS text messages\. If you haven't sent an SMS message from Amazon Cognito or any other AWS service before, Amazon SNS might place your account in the SMS sandbox\. We recommend that you send a test message to a verified phone number before you remove your account from the sandbox to production\. Additionally, if you plan to send SMS messages to US destination phone numbers, you must obtain an origination or Sender ID from Amazon Pinpoint\. To configure your Amazon Cognito user pool for SMS, see [SMS message settings for Amazon Cognito user pools](user-pool-sms-settings.md)\.

Amazon Cognito can automatically verify email addresses or mobile phone numbers\. To do this verification, Amazon Cognito sends a verification code or, for email, a verification link\. For email addresses, Amazon Cognito sends the code or link in an email message\. For phone numbers, Amazon Cognito sends the code in an SMS text message\.

Amazon Cognito must verify a phone number or email address to confirm users and help them to recover forgotten passwords\. Alternatively, you can automatically confirm users with the pre\-sign up Lambda trigger or use the [AdminConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminConfirmSignUp.html) API operation\. For more information, see [Signing up and confirming user accounts](signing-up-users-in-your-app.md)\.

The verification code or link is valid for 24 hours\.

If you choose to require verification for an email address or phone number, Amazon Cognito automatically sends the verification code or link when a user signs up\. If the user pool has a [Custom SMS sender Lambda trigger](user-pool-lambda-custom-sms-sender.md) or [Custom email Lambda trigger](user-pool-lambda-custom-email-sender.md) configured, that function is invoked instead\.

**Notes**  
Amazon SNS charges separately for SMS text messaging that it uses to verify phone numbers\. There is no charge to send verification codes to email addresses\. For information about Amazon SNS pricing, see [Worldwide SMS pricing](https://aws.amazon.com/sns/sms-pricing/)\. For the current list of countries where SMS messaging is available, see [Supported regions and countries](https://docs.aws.amazon.com/sns/latest/dg/sms_supported-countries.html)\. 
When you test actions in your app that generate email messages from Amazon Cognito, use a real email address that Amazon Cognito can reach without hard bounces\. For more information, see [Sending emails while testing your app](signing-up-users-in-your-app.md#managing-users-accounts-email-testing)\.
The forgotten password flow requires either the user's email or the user's phone number to verify the user\.

**Important**  
If a user signs up with both a phone number and an email address, and your user pool settings require verification of both attributes, Amazon Cognito sends a verification code to the phone through SMS\. Amazon Cognito hasn't yet verified the email address, so your app must call [GetUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html) to see if an email address awaits verification\. If it does require verification, the app must call [GetUserAttributeVerificationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUserAttributeVerificationCode.html) to initiate the email verification flow\. Then it must submit the verification code by calling [VerifyUserAttribute](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifyUserAttribute.html)\.

You can specify spend limits for an AWS account and for individual messages\. The limits apply only to the cost to send SMS messages\. For more information, see [Amazon SNS FAQs](http://aws.amazon.com/sns/faqs/)\.

Amazon Cognito sends SMS messages using Amazon SNS resources in either the AWS Region where you created the user pool or in a **Legacy Amazon SNS alternate Region** from the following table\. The exception is Amazon Cognito user pools in the Asia Pacific \(Seoul\) Region\. These user pools use your Amazon SNS configuration in the Asia Pacific \(Tokyo\) Region\. For more information, see [Step 1: Choose the AWS Region for Amazon SNS SMS messages](user-pool-sms-settings.md#sms-choose-a-region)\.


| Amazon Cognito Region | Legacy Amazon SNS alternate Region | 
| --- | --- | 
| US East \(Ohio\) | US East \(N\. Virginia\) | 
| Asia Pacific \(Mumbai\) | Asia Pacific \(Singapore\) | 
| Asia Pacific \(Seoul\) | Asia Pacific \(Tokyo\) | 
| Canada \(Central\) | US East \(N\. Virginia\) | 
| Europe \(Frankfurt\) | Europe \(Ireland\) | 
| Europe \(London\) | Europe \(Ireland\) | 

**Example: **If your Amazon Cognito user pool is in Asia Pacific \(Mumbai\), and you have increased your spend limit in ap\-southeast\-1, you might not want to request a separate increase in ap\-south\-1\. Instead, you can use your Amazon SNS resources in Asia Pacific \(Singapore\)\. 

## Authorizing Amazon Cognito to send SMS messages on your behalf<a name="user-pool-settings-verifications-iam-role-for-sms"></a>

To send SMS messages to your users on your behalf, Amazon Cognito needs your permission\. To grant that permission, you can create an AWS Identity and Access Management \(IAM\) role\. Under the **MFA and verifications** tab of the Amazon Cognito console, choose **Create role**\.