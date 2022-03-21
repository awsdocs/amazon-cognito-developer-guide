# SMS message settings for Amazon Cognito user pools<a name="user-pool-sms-settings"></a>

Some Amazon Cognito events for your user pool might cause Amazon Cognito to send SMS text messages to your users\. For example, if you configure your user pool to require phone verification, Amazon Cognito sends an SMS text message when a user signs up for a new account in your app or resets their password\. Depending on the action that initiates the SMS text message, the message contains a verification code, a temporary password, or a welcome message\.

Amazon Cognito uses Amazon Simple Notification Service \(Amazon SNS\) for delivery of SMS text messages\. If you are sending a text message through Amazon Cognito or Amazon SNS for the first time, Amazon SNS places you in a sandbox environment\. In the sandbox environment, you can test your applications for SMS text messages\. In the sandbox, messages can be sent only to verified phone numbers\. 

## Setting up SMS messaging for the first time in Amazon Cognito user pools<a name="user-pool-sms-settings-first-time"></a>

Amazon Cognito uses Amazon SNS to send SMS messages to your user pools\. You can also use a [Custom SMS sender Lambda trigger](user-pool-lambda-custom-sms-sender.md) to use your own resources to send SMS messages\. The first time that you set up Amazon SNS to send SMS text messages in a particular AWS Region, Amazon SNS places your AWS account in the SMS sandbox for that Region\.Amazon SNS uses the sandbox to prevent fraud and abuse and to meet compliance requirements\. When your AWS account is in the sandbox, Amazon SNS imposes some [restrictions](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox.html)\. For example, you can send text messages to a maximum of 10 phone numbers that you have verified with Amazon SNS\. While your AWS account remains in the sandbox, do not use your Amazon SNS configuration for applications that are in production\. When you're in the sandbox, Amazon Cognito can't send messages to your users' phone numbers\.

**To send SMS text messages to user pool users**

1. Choose the AWS Region for Amazon SNS SMS messages\.

1. Obtain an origination identity if you want to send SMS messages to US phone numbers\.

1. Confirm that you are in the SMS sandbox\.

1. Move your account out of Amazon SNS sandbox\.

1. Verify phone numbers for Amazon Cognito in Amazon SNS\.

1. Finish setting up the user pool in Amazon Cognito\.

### Step 1: Choose the AWS Region for Amazon SNS SMS messages<a name="sms-choose-a-region"></a>

In some AWS Regions, you can choose the Region that contains the Amazon SNS resources that you want to use for Amazon Cognito SMS messages\. In any AWS Region where Amazon Cognito is available, except for Asia Pacific \(Seoul\), you can use Amazon SNS resources in the AWS Region where you created your user pool\. To make your SMS messaging faster and more reliable when you have a choice of Regions, use Amazon SNS resources in the same Region as your user pool\.

Choose a Region for SMS resources in the **Configure message delivery** step of the new user pool wizard\. You can also choose **Edit** under **SMS** in the **Messaging** tab of an existing user pool\.

At launch, for some AWS Regions, Amazon Cognito sent SMS messages with Amazon SNS resources in an alternate Region\.  To set your preferred Region, use the `SnsRegion` parameter of the [SmsConfigurationType](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SmsConfigurationType.html) object for your user pool\. When you programmatically create an Amazon Cognito user pools resource in an **Amazon Cognito Region** from the following table and you do not provide an `SnsRegion` parameter, your user pool sends SMS messages with Amazon SNS resources in the corresponding **Legacy Amazon SNS alternate Region**\.

Amazon Cognito user pools in the Asia Pacific \(Seoul\) AWS Region must use your Amazon SNS configuration in the Asia Pacific \(Tokyo\) Region\.

You can send SMS messages for any **Amazon Cognito Region** in the following table with Amazon SNS resources in the corresponding legacy alternate AWS Regions\.


| Amazon Cognito Region | Legacy Amazon SNS alternate Region | 
| --- | --- | 
| US East \(Ohio\) | US East \(N\. Virginia\) | 
| Asia Pacific \(Mumbai\) | Asia Pacific \(Singapore\) | 
| Canada \(Central\) | US East \(N\. Virginia\) | 
| Europe \(Frankfurt\) | Europe \(Ireland\) | 
| Europe \(London\) | Europe \(Ireland\) | 

### Step 2: Obtain an origination identity to send SMS messages to US phone numbers<a name="user-pool-sms-settings-first-time-origination"></a>

If you plan to send SMS text messages to US phone numbers, you must obtain an origination identity, regardless of whether you build an SMS sandbox testing environment, or a production environment\.

Starting June 1, 2021, US carriers require an origination identity to send messages to US phone numbers\. If you don't already have an origination identity, you must get one\. To learn how to obtain an origination identity, see [ Requesting a number](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-request-number.html) in the *Amazon Pinpoint User Guide*\.

If you operate in the following AWS Regions, you must open an AWS Support ticket to obtain an origination identity\. For instructions, see [Requesting support for SMS messaging](https://docs.aws.amazon.com/sns/latest/dg/channels-sms-awssupport.html) in the *Amazon Simple Notification Service Developer Guide*\.
+ US East \(Ohio\)
+ Europe \(Stockholm\)
+ Middle East \(Bahrain\)
+ Europe \(Paris\)
+ South America \(São Paulo\)
+ US West \(N\. California\)

### Step 3: Confirm that you are in the SMS sandbox<a name="user-pool-sms-settings-first-time-confirm-sandbox"></a>

Use the following procedure to confirm that you are in the SMS sandbox\. Repeat for each AWS Region where you have production Amazon Cognito user pools\.

#### Review SMS sandbox status in the Amazon Cognito console<a name="check-that-you-are-in-the-sms-sandbox"></a>

------
#### [ Original console ]

**To confirm that you are in the SMS sandbox**

1. Open the Amazon Cognito console at [https://console\.aws\.amazon\.com/cognito](https://console.aws.amazon.com/cognito)\. If prompted, enter your AWS credentials\.

1. [Create a new user pool](cognito-user-pool-as-user-directory.md) or [edit an existing user pool](signing-up-users-in-your-app.md#verification-configure)\.

1. If your account is in the SMS sandbox, you will see the following message in Amazon Cognito:

   `You are currently in a Sandbox environment in [Amazon SNS](https://console.aws.amazon.com/sns/home).`

   If you don’t see this message, then someone has set up SMS messages in your account already\. Skip to [Step 6: Complete user pool setup in Amazon Cognito](#user-pool-sms-settings-first-time-finish-user-pool)\.

1. In the message to open the Amazon SNS console in a new tab, choose the [Amazon SNS](https://console.aws.amazon.com/sns/home) link\.

1. Verify that you are in the sandbox environment\. The console message indicates your sandbox status and AWS Region, as follows:

   `This account is in the SMS sandbox in US East (N. Virginia).`

------
#### [ New console ]

**To confirm that you are in the SMS sandbox**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Messaging** tab\.

1. In the **SMS configuration** section, expand **Move to Amazon SNS production environment**\. If your account is in the SMS sandbox, you will see the following message:

   `You are currently in the SMS Sandbox and cannot send SMS messages to unverified numbers.`

   If you don’t see this message, then someone has set up SMS messages in your account already\. Skip to [Step 6: Complete user pool setup in Amazon Cognito](#user-pool-sms-settings-first-time-finish-user-pool)\.

1. Choose the [Amazon SNS](https://console.aws.amazon.com/sns/home) link in the message\. This opens the Amazon SNS console in a new tab\.

1. Verify that you are in the sandbox environment\. The console message indicates your sandbox status and AWS Region, as follows:

   `This account is in the SMS sandbox in US East (N. Virginia).`

------

### Step 4: Move your account out of Amazon SNS sandbox<a name="user-pool-sms-settings-first-time-out-sandbox"></a>

If you are testing your app and you only need to send SMS messages to phone numbers that your administrators can verify, skip this step and proceed to step 5\.

To use your app in production, move your account out of the SMS sandbox and into production\. After you have configured an origination identity in the AWS Region that contains the Amazon SNS resources that you want Amazon Cognito to use, you can verify US phone numbers while your AWS account remains in the SMS sandbox\. When your Amazon SNS environment is in production, you don't have to verify user phone numbers in Amazon SNS to send SMS messages to your users\. 

For detailed instructions, see [Moving Out of the SMS Sandbox](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox-moving-to-production.html) in the *Amazon Simple Notification Service Developer Guide*\.

### Step 5: Verify phone numbers for Amazon Cognito in Amazon SNS<a name="user-pool-sms-settings-first-time-verify-numbers"></a>

If you have moved your account out of the SMS sandbox, skip this step and proceed to step 6\.

When you are in the SMS sandbox, you can send messages to any phone number that you have verified with Amazon SNS\. 

To verify a phone number, do the following:

1. Add a **Sandbox destination phone number** in the **Text messaging \(SMS\)** section of the Amazon SNS console\.

1. Receive an SMS message with a code at the phone number that you provided\.

1. Enter the **Verification code** from the SMS message in the Amazon SNS console\.

For detailed instructions, see [Adding and verifying phone numbers in the SMS sandbox](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox-verifying-phone-numbers.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Note**  
Amazon SNS limits the number of destination phone numbers that you can verify while you are in the SMS sandbox\. See [SMS sandbox](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox.html) in the *Amazon Simple Notification Service Developer Guide*\.

### Step 6: Complete user pool setup in Amazon Cognito<a name="user-pool-sms-settings-first-time-finish-user-pool"></a>

Return to the browser tab where you were [creating](cognito-user-pool-as-user-directory.md) or [editing](signing-up-users-in-your-app.md#verification-configure) your user pool\. Complete the procedure\.