# Customizing the SMS message<a name="cognito-user-pool-settings-SMS-message-customization"></a>

**Note**  
In the new Amazon Cognito console experience, you can customize SMS messages in the **Messaging** tab under the **Message templates** heading\.

You can customize the SMS message for multi\-factor authentication \(MFA\) by editing the template under the **Do you want to customize your SMS messages?** heading\.

**Important**  
Your custom message must contain the `{####}` placeholder\. This placeholder is replaced with the authentication code before the message is sent\.

The maximum length for the message, including the authentication code, is 140 UTF\-8 characters\.

## Customizing SMS verification messages<a name="cognito-user-pool-settings-SMS-verification-message-customization"></a>

You can customize the SMS message for phone number verifications by editing the template under the **Do you want to customize your SMS verification messages?** heading\.

**Important**  
Your custom message must contain the `{####}` placeholder\. This placeholder is replaced with the verification code before the message is sent\.

The maximum length for the message, including the verification code, is 140 UTF\-8 characters\.