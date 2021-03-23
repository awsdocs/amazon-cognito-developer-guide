# Customizing Your Email Address<a name="cognito-user-pool-settings-email-address-customization"></a>

By default, the email messages that Amazon Cognito sends to users in your user pools come from **no\-reply@verificationemail\.com**\. You can specify custom FROM email addresses and REPLY\-TO email addresses to be used instead of **no\-reply@verificationemail\.com**\.

To customize the FROM email address, in the console, choose a userpool\. Next, choose **Message customizations\.** In **Message customizations,** choose a ** SES Region **\. In the **FROM email address ARN** field enter your email address\. Verify the Amazon Simple Email Service identity by choosing the link after the **FROM email address ARN field\.** For more information, see [Verifying Email Addresses and Domains in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the *Amazon Simple Email Service Developer Guide*\.

To customize the REPLY\-TO email address, enter a valid email address in the **REPLY\-TO email address** field\.