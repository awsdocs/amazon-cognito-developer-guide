# Customizing email verification messages<a name="cognito-user-pool-settings-email-verification-message-customization"></a>

You can choose one of two methods for your user pool's users to verify their email address with Amazon Cognito\. They can enter a code, or click on a link, sent to them in an email message\.

**Note**  
In the new Amazon Cognito console experience, you can customize email verification messages in the **Messaging** tab under the **Message templates** heading\.

You can customize the email subject and message content for email address verification messages by editing the template under the **Do you want to customize your email verification messages?** heading\.

**Important**  
If you have chosen code as the verification type, your custom message must contain the `{####}` placeholder\. This placeholder is replaced with the verification code when the message is sent\.

The maximum length for the message, including the verification code \(if present\), is 20,000 UTF\-8 characters\. You can use HTML tags in this message to format the contents\.