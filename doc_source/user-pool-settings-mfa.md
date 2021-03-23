# Adding Multi\-Factor Authentication \(MFA\) to a User Pool<a name="user-pool-settings-mfa"></a>

Multi\-factor authentication \(MFA\) increases security for your app by adding another authentication method and not relying solely on user name and password\. You can choose to use SMS text messages or time\-based one\-time \(TOTP\) passwords as second factors in signing in your users\.

With adaptive authentication, you can configure your user pool to require second factor authentication in response to an increased risk level\. To add adaptive authentication to your user pool, see [Adding Advanced Security to a User Pool](cognito-user-pool-settings-advanced-security.md)\.

When multi\-factor authentication \(MFA\) is set to `required`for a user pool, all users must complete MFA to sign in\. Each user needs at least one MFA factor such as SMS or TOTP setup to sign in\. To avoid having users blocked from signing in when MFA is `required`, you must include the MFA setup in user onboarding\.

If you enable SMS as an MFA factor, you can require phone numbers and have them verified during sign\-up\. If you have MFA set to `required` and only support SMS as a factor, most users will need to have a phone number\. Users without phone numbers will need your support to add a phone number to their profile before they can sign in\. You can use unverified phone numbers for SMS MFA\. These numbers will have a verified status after multi\-factor authentication succeeds\.

Setting up users in user pools for TOTP tokens occurs during initial sign\-in\. The setting to enable or disable TOTP as an MFA factor for a user pool controls whether users can set up TOTP for themselves\. If your users have set up TOTP, they can use it for MFA even if TOTP is later disabled for the user pool\.

**Topics**
+ [Prerequisites](#user-pool-settings-mfa-prerequisites)
+ [Configuring Multi\-Factor Authentication](#user-pool-configuring-mfa)
+ [SMS Text Message MFA](user-pool-settings-mfa-sms-text-message.md)
+ [TOTP Software Token MFA](user-pool-settings-mfa-totp.md)

## Prerequisites<a name="user-pool-settings-mfa-prerequisites"></a>

Before you begin, you need the following:
+ You can only choose MFA as **Required** in the Amazon Cognito user pool console when you initially create a user pool\. The [SetUserPoolMfaConfig](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUserPoolMfaConfig.html) API operation can be used to set MFA to `required` for existing user pools\.
+ Phone numbers must be verified if MFA is enabled and **SMS text message** is chosen as a second factor\.
+ Advanced security features require that MFA is enabled, and set as optional in the Amazon Cognito user pool console\. For more information, see [Adding Advanced Security to a User Pool](cognito-user-pool-settings-advanced-security.md)\.

## Configuring Multi\-Factor Authentication<a name="user-pool-configuring-mfa"></a>

You can configure MFA in the Amazon Cognito console\.

**To configure MFA in the Amazon Cognito console**

1. From the left navigation bar, choose **MFA and verifications**\.

1. Choose whether MFA is **Off**, **Optional**, or **Required**\.  
![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. Choose **Optional** to enable MFA on a per\-user basis or if you are using the risk\-based adaptive authentication\. For more information on adaptive authentication, see [Adding Advanced Security to a User Pool](cognito-user-pool-settings-advanced-security.md)\.

1. Choose which second factors to support in your app\. Your users can use **SMS text message** or **Time\-based One\-time Password** as a second factor\. We recommend using TOTP, which allows SMS to be used as a password recovery mechanism rather than as an authentication factor\.

1. If you're using SMS text messages as a second factor and you don't have an IAM role defined with this permission, then you can create one in the console\. Choose **Create role** to create an IAM role that allows Amazon Cognito to send SMS messages to your users on your behalf\. For more information, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)\.

1. Choose **Save changes**\.