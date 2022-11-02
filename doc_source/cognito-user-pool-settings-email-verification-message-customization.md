# Customizing email verification messages<a name="cognito-user-pool-settings-email-verification-message-customization"></a>

To verify the email address of a user in your user pool with Amazon Cognito, you can send the user an email message with a link that they can select, or you can send them a code that they can enter\.

**Note**  
In the new Amazon Cognito console, you can customize email verification messages under the **Message Templates** heading in the **Messaging** tab of your user pool\.

To customize the email subject and message content for email address verification messages, edit the template under the **Do you want to customize your email verification messages?** heading\.

**Important**  
If you choose `code` as the verification type, your custom message must contain the `{####}` placeholder\. When you send the message, the verification code replaces this placeholder\. If you choose `link` as the verification type, your custom message must include a placeholder in the format `{##Verify Your Email##}`\. A verification link titled *Verify Your Email* replaces this placeholder\.

The link for an email verification message directs your user to a URL like the following example\.

```
https://<your user pool domain>/confirmUser/?client_id=abcdefg12345678&user_name=emailtest&confirmation_code=123456
```

The maximum length for the message, including the verification code \(if present\), is 20,000 UTF\-8 characters\. You can use HTML tags in this message to format the contents\.