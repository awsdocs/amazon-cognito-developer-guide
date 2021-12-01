# SMS message settings for Amazon Cognito user pools<a name="user-pool-sms-settings"></a>

Some Amazon Cognito events for your user pool might cause Amazon Cognito to send SMS text messages to your users\. For example, if you configure your user pool to require phone verification, Amazon Cognito sends an SMS text message when a user signs up for a new account in your app or resets their password\. Depending on the action that initiates the SMS text message, the message contains a verification code, a temporary password, or a welcome message\.

Amazon Cognito uses Amazon Simple Notification Service \(SNS\) for delivery of SMS text messages\. If this is the first time that you are sending a text message through Amazon Cognito or Amazon SNS, you will be placed in a sandbox environment in Amazon SNS\. This will allow you to test your applications for SMS text messages\. In the sandbox, messages can be sent only to verified phone numbers\. 

## Setting up SMS messages for the first time in Amazon Cognito user pools<a name="user-pool-sms-settings-first-time"></a>

Amazon Cognito uses Amazon SNS to send SMS messages to your user pools\. The first time that you set up Amazon SNS to send SMS text messages, your AWS account is placed in the Amazon SNS sandbox\. Amazon SNS uses the sandbox to prevent fraud and abuse and to meet compliance requirements\. In the sandbox, Amazon SNS imposes some [restrictions](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox.html)\. For example, you can send text messages to up to 10 phone numbers that you have verified with Amazon SNS\. While your AWS account remains in the sandbox, do not use your Amazon SNS configuration for applications that are in production\. When you're in the sandbox, Amazon Cognito can't send messages to your users' phone numbers\.

To send SMS text messages to user pool users in production for the first time, you must complete the following tasks:

1\. Confirm that you are in the SMS sandbox

2\. Verify phone numbers for Amazon Cognito in Amazon SNS

3\. Obtain an origination identity for sending SMS messages to U\.S\. phone numbers

4\. Move your account out of Amazon SNS sandbox

5\. Finish setting up the user pool in Amazon Cognito

### STEP 1: Confirm that you are in the SMS sandbox<a name="user-pool-sms-settings-first-time-confirm-sandbox"></a>

Use the following procedure to check that you are in the SMS sandbox\.

------
#### [ Original console ]

**To confirm that you are in the SMS sandbox**

1. Open the Amazon Cognito console at [https://console\.aws\.amazon\.com/cognito](https://console.aws.amazon.com/cognito)\. If prompted, enter your AWS credentials\.

1. [Create a new user pool](cognito-user-pool-as-user-directory.md) or [edit an existing user pool](signing-up-users-in-your-app.md#verification-configure)\.

1. If your account is in the SMS sandbox, you will see the following message in Amazon Cognito:

   `You are currently in a Sandbox environment in [Amazon SNS](https://console.aws.amazon.com/sns/home).`

   If you don’t see this message, then someone already performed the necessary steps to set up SMS messages for the first time in your account\. Skip to [STEP 5: Complete user pool setup in Amazon Cognito](#user-pool-sms-settings-first-time-finish-user-pool)\.

1. Choose the [Amazon SNS](https://console.aws.amazon.com/sns/home) link in the message to open the Amazon SNS console in a new tab\.

1. Verify that you are in the sandbox environment\. The console message indicates your sandbox status and AWS Region\. For example:

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

   If you don’t see this message, then someone already performed the necessary steps to set up SMS messages for the first time in your account\. Skip to [STEP 5: Complete user pool setup in Amazon Cognito](#user-pool-sms-settings-first-time-finish-user-pool)\.

1. Choose the [Amazon SNS](https://console.aws.amazon.com/sns/home) link in the message to open the Amazon SNS console in a new tab\.

1. Verify that you are in the sandbox environment\. The console message indicates your sandbox status and AWS Region\. For example:

   `This account is in the SMS sandbox in US East (N. Virginia).`

------

In most AWS Regions, SMS messages from your user pool are routed through Amazon SNS in the same region\. SMS messages in the following Amazon Cognito Regions are rerouted through the corresponding supported Amazon SNS Regions\. 


| Amazon Cognito Regions | Supported Amazon SNS Regions | 
| --- | --- | 
| US East \(Ohio\) | US East \(N\. Virginia\) | 
| Asia Pacific \(Mumbai\) | Asia Pacific \(Singapore\) | 
| Asia Pacific \(Seoul\) | Asia Pacific \(Tokyo\) | 
| Canada \(Central\) | US East \(N\. Virginia\) | 
| Europe \(Frankfurt\) | Europe \(Ireland\) | 
| Europe \(London\) | Europe \(Ireland\) | 

### STEP 2: Verify phone numbers for Amazon Cognito in Amazon SNS<a name="user-pool-sms-settings-first-time-verify-numbers"></a>

To verify SMS destination phone numbers for testing with your application, you must add destination phone numbers to Amazon SNS and then verify the numbers\. For detailed instructions, see [Adding and verifying phone numbers in the SMS sandbox](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox-verifying-phone-numbers.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Note**  
There is a limit to the number of destination phone numbers you can add to the sandbox\. For details, see [SMS sandbox](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox.html) in the *Amazon Simple Notification Service Developer Guide*\.

### STEP 3: Obtain an origination identity for sending SMS messages to US phone numbers<a name="user-pool-sms-settings-first-time-origination"></a>

If you plan to send SMS text messages to U\.S\. phone numbers, you must obtain an origination identity\. 

Starting June 1, 2021, U\.S\. carriers require an origination identity to send messages to U\.S\. phone numbers\. If you do not already have an origination identity, you must get one\. To learn how to obtain an origination identity, see [ Requesting a number](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-request-number.html) in the *Amazon Pinpoint User Guide*\.

If you operate in the following AWS Regions, you must open an AWS Support ticket to obtain an origination identity\. For instructions, see [Requesting support for SMS messaging](https://docs.aws.amazon.com/sns/latest/dg/channels-sms-awssupport.html) in the *Amazon Simple Notification Service Developer Guide*\.
+ Europe \(Stockholm\)
+ Middle East \(Bahrain\)
+ Europe \(Paris\)
+ South America \(São Paulo\)
+ US West \(N\. California\)

### STEP 4: Move your account out of Amazon SNS sandbox<a name="user-pool-sms-settings-first-time-out-sandbox"></a>

When your account is in the SMS sandbox in Amazon SNS, Amazon Cognito can send SMS text messages to only verified phone numbers and not to your end users\.

To send SMS messages to end users, you must move your account out of the sandbox and into production\. For detailed instructions, see [Moving Out of the Amazon SNS Sandbox](https://docs.aws.amazon.com/sns/latest/dg/sns-sms-sandbox.html) in the *Amazon Simple Notification Service Developer Guide*\.

### STEP 5: Complete user pool setup in Amazon Cognito<a name="user-pool-sms-settings-first-time-finish-user-pool"></a>

Return to the browser tab where you were [creating](cognito-user-pool-as-user-directory.md) or [editing](signing-up-users-in-your-app.md#verification-configure) your user pool\. Complete the procedure\. 