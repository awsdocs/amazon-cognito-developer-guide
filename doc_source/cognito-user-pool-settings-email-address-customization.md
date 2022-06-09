# Customizing your email address<a name="cognito-user-pool-settings-email-address-customization"></a>

By default, Amazon Cognito sends email messages to users in your user pools from the address **no\-reply@verificationemail\.com**\. You can choose to specify custom FROM and REPLY\-TO email addresses instead of **no\-reply@verificationemail\.com**\.

To customize the FROM and REPLY\-TO email addresses in the AWS console:

------
#### [ Original console ]

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and choose a user pool\.

1. Choose **Message customizations**\.

1. In **Message customizations**\. choose an **SES Region**\.

1. In the **FROM email address ARN** field, choose your verified email address from the list\. Verify the Amazon Simple Email Service identity by choosing the [Verify an SES identity](https://console.aws.amazon.com/ses/home#verified-senders-email:)link below the **FROM email address ARN** field\. For more information, see [Verifying email addresses and domains in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the *Amazon Simple Email Service Developer Guide*\.

1. To customize the REPLY\-TO email address, enter a valid email address in the **REPLY\-TO email address** field\.

------
#### [ New console ]

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Messaging** tab\. Under **Email**, choose **Edit**\.

1. Choose an **SES Region**\.

1. Choose a **FROM email address** from the list of email addresses you have verified with Amazon SES in the **SES Region** you selected\. To use an email address from a verified domain, configure email settings in the AWS Command Line Interface or the AWS API\. For more information, see [Verifying email addresses and domains in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the *Amazon Simple Email Service Developer Guide*\.

1. Choose a **Configuration set** from the list of configuration sets in your chosen **SES Region**\.

1. Enter a friendly **FROM sender name** for your email messages, in the format `John Stiles <johnstiles@example.com>`\.

1. To customize the REPLY\-TO email address, enter a valid email address in the **REPLY\-TO email address** field\.

------