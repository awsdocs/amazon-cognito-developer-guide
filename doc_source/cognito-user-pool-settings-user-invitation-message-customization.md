# Customizing User Invitation Messages<a name="cognito-user-pool-settings-user-invitation-message-customization"></a>

You can customize the user invitation message that Amazon Cognito sends to new users via SMS or email by editing the templates under the **Do you want to customize your user invitation messages?** heading\.

**Important**  
Your custom message must contain the `{username}` and `{####}` placeholders, which are replaced with the user's username and password before the message is sent\.

For SMS, the maximum length is 140 UTF\-8 characters, including the verification code\. For email, the maximum length for the message is 20,000 UTF\-8 characters, including the verification code\. HTML tags can be used in these emails\.