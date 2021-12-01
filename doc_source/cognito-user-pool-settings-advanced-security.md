# Adding advanced security to a user pool<a name="cognito-user-pool-settings-advanced-security"></a>

After you create your user pool, you have access to **Advanced security** on the navigation bar in the Amazon Cognito console\. You can turn the user pool advanced security features on, and customize the actions that are taken in response to different risks\. Or you can use audit mode to gather metrics on detected risks without taking action\. In audit mode, the advanced security features publish metrics to Amazon CloudWatch\. See [Viewing advanced security metrics](user-pool-settings-viewing-advanced-security-metrics.md)\.

**Note**  
Additional pricing applies for Amazon Cognito advanced security features\. See the [Amazon Cognito pricing page](https://aws.amazon.com/cognito/pricing/)\.

**Topics**
+ [Prerequisites](#cognito-user-pool-advanced-security-prerequisites)
+ [Configuring advanced security features](#cognito-user-pool-configure-advanced-security)
+ [Checking for compromised credentials](cognito-user-pool-settings-compromised-credentials.md)
+ [Using adaptive authentication](cognito-user-pool-settings-adaptive-authentication.md)
+ [Viewing advanced security metrics](user-pool-settings-viewing-advanced-security-metrics.md)
+ [Enabling user pool advanced security from your app](user-pool-settings-viewing-advanced-security-app.md)

## Prerequisites<a name="cognito-user-pool-advanced-security-prerequisites"></a>

Before you begin, you need:
+ A user pool with an app client\. For more information, see [Getting started with user pools](getting-started-with-cognito-user-pools.md)\.
+ Set multi\-factor authentication \(MFA\) to **Optional** in the Amazon Cognito console to use the risk\-based adaptive authentication feature\. For more information, see [Adding multi\-factor authentication \(MFA\) to a user pool](user-pool-settings-mfa.md)\.
+ If you're using email for notifications, go to the [Amazon SES console](https://console.aws.amazon.com/ses/home) to configure and verify an email address or domain to use with your notification emails\. For more information about Amazon SES, see [Verifying Identities in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html)\.

## Configuring advanced security features<a name="cognito-user-pool-configure-advanced-security"></a>

You can configure Amazon Cognito advanced security features in the AWS Management Console\.

------
#### [ Original console ]

**To configure advanced security for a user pool**

1. From the left navigation bar, choose **Advanced security**\.

1. For **Do you want to enable advanced security features for this user pool?**, choose **Yes** to enable advanced security\. Or choose **Audit only** to gather information, and send user pool data to Amazon CloudWatch\. 

   We recommend keeping the advanced security features in audit mode for two weeks before enabling actions\. This allows Amazon Cognito to learn the usage patterns of your app users\.

1. From the drop\-down list, choose **What app client do you want to customize settings for?**\. The default is to leave your settings as global for all app clients\.

1. For **Which action do you want to take with the compromised credentials?**, choose **Allow** or **Block use**\. 

1. Choose **Customize when compromised credentials are blocked** to select which events should trigger compromised credentials checks:
   + Sign\-in
   + Sign\-up
   + Password change

1. Choose how to respond to malicious sign\-in attempts under **How do you want to use adaptive authentication for sign\-in attempts rated as low, medium and high risk?**\. You can allow or block the sign\-in attempt, or require additional challenges before allowing the sign\-in\.

   To send email notifications when anomalous sign\-in attempts are detected, choose **Notify users** \.  
![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. If you chose **Notify users** in the previous step, then you can customize the email notification messages by using the **Notification message customization** form:  
![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. Choose **Customize** to customize adaptive authentication notifications with both HTML and plaintext email versions\. To learn more about email templates, see [Message templates](cognito-user-pool-settings-message-templates.md)\.

1. Type any IP addresses that you want to **Always allow**, or **Always block**, regardless of the advanced security risk assessment\. Specify the IP address ranges in [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation) \(such as 192\.168\.100\.0/24\)\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To configure advanced security for a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **App integration** tab\. Locate **Advanced security** and choose **Enable**\. If you enabled advanced security earlier, choose **Edit**\.

1. Select **Full function** to configure advanced security responses to Compromised credentials and Adaptive authentication\. Select **Audit only** to gather information and send user pool data to Amazon CloudWatch\. Advanced security pricing applies in both **Audit only** and **Full function** mode\. For more information, see [Amazon Cognito Pricing](https://aws.amazon.com/cognito/pricing/)\.

   We recommend keeping the advanced security features in audit mode for two weeks before enabling actions\. This allows Amazon Cognito to learn the usage patterns of your app users\.

1. If you selected **Audit only**, choose **Save changes**\. If you selected **Full function**:

   1. Select whether you will take **Custom** action or use or **Cognito defaults** to respond to suspected **Compromised credentials**\. **Cognito defaults** are:

      1. Detect compromised credentials on **Sign\-in**, **Sign\-up**, and **Password change**\.

      1. Respond to compromised credentials with the action **Block sign\-in**\.

   1. If you selected **Custom** actions for **Compromised credentials**, choose the user pool actions that Amazon Cognito will use for **Event detection** and the **Compromised credentials responses** that you would like Amazon Cognito to take\. You can **Block sign\-in** or **Allow sign\-in** with suspected compromised credentials\.

   1. Choose how to respond to malicious sign\-in attempts under **Adaptive authentication**\. Select whether you will take **Custom** action or use or **Cognito defaults** to respond to suspected malicious activity\. When you select**Cognito defaults**, Amazon Cognito blocks sign\-in at all risk levels and does not notify the user\.

   1. If you selected **Custom** actions for **Adaptive authentication**, choose the **Automatic risk response** actions that Amazon Cognito will take in response to detected risks based on severity level\. When you assign a response to a level of risk, you can't assign a less\-restrictive response to a higher level of risk\. You can assign the following responses to risk levels:

      1. **Allow sign\-in** \- Take no preventative action\.

      1. **Optional MFA** \- If the user has MFA configured, Amazon Cognito will always require the user to provide an additional SMS or time\-based one\-time password \(TOTP\) factor when they sign in\. If the user does not have MFA configured, they can continue signing in normally\.

      1. **Require MFA** \- If the user has MFA configured, Amazon Cognito will always require the user to provide an additional SMS or TOTP factor when they sign in\. If the user does not have MFA configured, Amazon Cognito will prompt them to set up MFA\. Before you automatically require MFA for your users, ensure that you have configured a mechanism in your app to capture phone numbers for SMS MFA, or to register authenticator apps for TOTP MFA\.

      1. **Block sign\-in** \- Prevent the user from signing in\.

      1. **Notify user** \- Send an email message to the user with information about the risk that Amazon Cognito detected and the response you have taken\. You can customize email templates for the messages you send\.

1. If you chose **Notify user** in the previous step, you can customize your email delivery settings and email templates for adaptive authentication\.

   1. Under **Email configuration**, choose the **SES Region**, **FROM email address**, **FROM sender name**, and **REPLY\-TO email address** that you want to use with adaptive authentication\. For more information on integrating your user pool email messages with Amazon Simple Email Service, see [Email settings for Amazon Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-email.html)\.

   1. Expand **Email templates** to customize adaptive authentication notifications with both HTML and plaintext email versions\. To learn more about email templates, see [Message templates](cognito-user-pool-settings-message-templates.md)\.

1. Expand **IP address exceptions** to create an **Always\-allow** or an **Always\-block** list of IPv4 or IPv6 address renges that will always be allowed or blocked, regardless of the advanced security risk assessment\. Specify the IP address ranges in [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation) \(such as 192\.168\.100\.0/24\)\.

1. Choose **Save changes**\.

------