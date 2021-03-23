# Customizing Email Verification Messages<a name="cognito-user-pool-settings-email-verification-message-customization"></a>

You can choose the verification type for email verifications: code or link\.

You can customize the email subject and message for email address verifications by editing the template under the **Do you want to customize your email verification messages?** heading\.

**Important**  
If you have chosen code as the verification type, your custom message must contain the `{####}` placeholder, which is replaced with the verification code before the message is sent\.

The maximum length for the message is 20,000 UTF\-8 characters, including the verification code \(if present\)\. HTML tags can be used in these emails\.