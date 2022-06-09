# Adding MFA to a user pool<a name="user-pool-settings-mfa"></a>

Multi\-factor authentication \(MFA\) increases security for your app\. It adds a *something you have* authentication factor to the *something you know* factor of user name and password\. You can choose SMS text messages or time\-based one\-time passwords \(TOTP\) as second factors to sign in your users\.

**Note**  
The first time that a new user signs in to your app, Amazon Cognito issues OAuth 2\.0 tokens, even if your user pool requires MFA\. The second authentication factor when your user signs in for the first time is their confirmation of the verification message that Amazon Cognito sends to them\. If your user pool requires MFA, Amazon Cognito prompts your user to register an additional sign\-in factor to use during each sign\-in attempt after the first\.

With adaptive authentication, you can configure your user pool to require second factor authentication in response to an increased risk level\. To add adaptive authentication to your user pool, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

When you set MFA to `required` for a user pool, all users must complete MFA to sign in\. To sign in, each user must set up at least one MFA factor, such as SMS or TOTP\. When you set MFA to `required`, you must include the MFA setup in user onboarding so that your user pool permits them to sign in\.

If you activate SMS as an MFA factor, you can require that users provide phone numbers and have your users verify them during sign\-up\. If you have set MFA to `required` and only support SMS as a factor, users will need to provide phone numbers\. Users without phone numbers need your support to add a phone number to their profile before they can sign in\. You can use unverified phone numbers for SMS MFA\. These numbers will receive verified status after MFA succeeds\.

If you have set MFA to `required` and you activated SMS and TOTP as supported verification methods, Amazon Cognito prompts new users without phone numbers to set up TOTP MFA\. If you have set MFA to `required` and the only MFA method you activated is TOTP, Amazon Cognito prompts all new users to set up TOTP MFA the second time they sign in\. Amazon Cognito generates a challenge to set up TOTP MFA in response to [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API operations\. 

**Topics**
+ [Prerequisites](#user-pool-settings-mfa-prerequisites)
+ [Configuring multi\-factor authentication](#user-pool-configuring-mfa)
+ [SMS text message MFA](user-pool-settings-mfa-sms-text-message.md)
+ [TOTP software token MFA](user-pool-settings-mfa-totp.md)

## Prerequisites<a name="user-pool-settings-mfa-prerequisites"></a>

Before you set up MFA, consider the following:
+ In the legacy Amazon Cognito console, you can only set MFA as **Required** when you initially create a user pool\. Switch to the new console or use the [SetUserPoolMfaConfig](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUserPoolMfaConfig.html) API operation to set MFA to `required` for existing user pools\.
+ When you activate MFA in your user pool and choose **SMS text message** as a second factor, you can send SMS messages to a phone number attribute that you haven't verified in Amazon Cognito\. After your user completes SMS MFA, Amazon Cognito sets their `phone_number_verified` attribute to `true`\.
+ If your account is in the SMS sandbox in the AWS Region that contains the Amazon Simple Notification Service \(Amazon SNS\) resources for your user pool, you must verify phone numbers in Amazon SNS before you can send an SMS message\. For more information, see [SMS message settings for Amazon Cognito user pools](user-pool-sms-settings.md)\.
+ Advanced security features require that you activate MFA and set it as optional in the Amazon Cognito user pool console\. For more information, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

## Configuring multi\-factor authentication<a name="user-pool-configuring-mfa"></a>

You can configure MFA in the Amazon Cognito console\.

------
#### [ Original console ]

**To configure MFA in the Amazon Cognito console**

1. From the left navigation bar, choose **MFA and verifications**\.

1. Choose whether MFA is **Off**, **Optional**, or **Required**\.  
![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. Choose **Optional** to activate MFA on a per\-user basis or if you are using the risk\-based adaptive authentication\. For more information about adaptive authentication, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

1. Choose which second factors to support in your app\. Your users can use **SMS text message** or **Time\-based One\-time Password** as a second factor\. We recommend that you use TOTP so that you can use SMS as a password recovery mechanism rather than as an authentication factor\.

1. If you use SMS text messages as a second factor and you don't have an IAM role defined with this permission, then you can create one in the console\. Choose **Create role** to create an IAM role that allows Amazon Cognito to send SMS messages to your users for you\. For more information, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To configure MFA in the Amazon Cognito console**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Multi\-factor authentication** and choose **Edit**

1. Choose the **MFA enforcement** method that you want to use with your user pool\.

   1. **Require MFA**\. All users in your user pool must sign in with an additional SMS code or time\-based one\-time password \(TOTP\) factor\.

   1. **Optional MFA** \- You can give your users the option to register an additional sign\-in factor but still permit users who haven't configured MFA to sign in\. If you use adaptive authentication, choose this option\. For more information about adaptive authentication, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

   1. **No MFA**\. Your users can't register an additional sign\-in factor\.

1. Choose the **MFA methods** that you support in your app\. You can set **SMS message** or TOTP\-generating **Authenticator apps** as a second factor\. We recommend that you implement TOTP\-based MFA so that account recovery can use SMS messages\.

1. If you use SMS text messages as a second factor and you haven't configured an IAM role to use with Amazon Simple Notification Service \(Amazon SNS\) for SMS messages, create one in the console\. In the **Messaging** tab for your user pool, locate **SMS** and choose **Edit**\. You can also use an existing role that allows Amazon Cognito to send SMS messages to your users for you\. For more information, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)\.

1. Choose **Save changes**\.

------