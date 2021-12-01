# Configuring SMS and email verification messages and user invitation messages<a name="cognito-user-pool-settings-message-customizations"></a>

**Note**  
In the new Amazon Cognito console experience, you can customize messages in the **Messaging** tab under the **Message templates** heading\.

In the **Message customizations** tab, you can customize:
+ Your SMS text message multi\-factor authentication \(MFA\) message
+ Your SMS and email verification messages
+ The verification type for email—code or link
+ Your user invitation messages
+ FROM and REPLY\-TO email addresses for emails going through your user pool

**Note**  
The SMS and email verification message templates only appear if you have chosen to require phone number and email verification in the **Verifications** tab\. Similarly, the SMS MFA message template only appears if the MFA setting is **required** or **optional**\.

**Topics**
+ [Message templates](cognito-user-pool-settings-message-templates.md)
+ [Customizing the SMS message](cognito-user-pool-settings-SMS-message-customization.md)
+ [Customizing email verification messages](cognito-user-pool-settings-email-verification-message-customization.md)
+ [Customizing user invitation messages](cognito-user-pool-settings-user-invitation-message-customization.md)
+ [Customizing your email address](cognito-user-pool-settings-email-address-customization.md)
+ [Authorizing Amazon Cognito to send Amazon SES email on your behalf \(from a custom FROM email address\)](cognito-user-pool-settings-ses-authorization-to-send-email.md)