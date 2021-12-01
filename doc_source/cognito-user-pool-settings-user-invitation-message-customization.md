# Customizing user invitation messages<a name="cognito-user-pool-settings-user-invitation-message-customization"></a>

**Note**  
In the new Amazon Cognito console experience, you can customize invitation messages in the **Messaging** tab under the **Message templates** heading\.

You can customize the user invitation message that Amazon Cognito sends to new users by SMS or email message by editing the templates under the **Do you want to customize your user invitation messages?** heading\.

**Important**  
Your custom message must contain the `{username}` and `{####}` placeholders\. These placeholders are replaced with the user's username and password before the message is sent\.

For SMS, the maximum length for the message, including the verification code, is 140 UTF\-8 characters\. For email, the maximum length for the message, including the verification code, is 20,000 UTF\-8 characters\. You may use HTML tags in your email messages to format the contents\.