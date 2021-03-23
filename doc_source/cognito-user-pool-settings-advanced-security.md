# Adding Advanced Security to a User Pool<a name="cognito-user-pool-settings-advanced-security"></a>

After you create your user pool, you have access to **Advanced security** on the navigation bar in the Amazon Cognito console\. You can turn the user pool advanced security features on, and customize the actions that are taken in response to different risks\. Or you can use audit mode to gather metrics on detected risks without taking action\. In audit mode, the advanced security features publish metrics to Amazon CloudWatch\. See [Viewing Advanced Security Metrics](user-pool-settings-viewing-advanced-security-metrics.md)\.

**Note**  
Additional pricing applies for Amazon Cognito advanced security features\. See the [Amazon Cognito pricing page](https://aws.amazon.com/cognito/pricing/)\.

**Topics**
+ [Prerequisites](#cognito-user-pool-advanced-security-prerequisites)
+ [Configuring Advanced Security Features](#cognito-user-pool-configure-advanced-security)
+ [Checking for Compromised Credentials](cognito-user-pool-settings-compromised-credentials.md)
+ [Using Adaptive Authentication](cognito-user-pool-settings-adaptive-authentication.md)
+ [Viewing Advanced Security Metrics](user-pool-settings-viewing-advanced-security-metrics.md)
+ [Enabling User Pool Advanced Security from Your App](user-pool-settings-viewing-advanced-security-app.md)

## Prerequisites<a name="cognito-user-pool-advanced-security-prerequisites"></a>

Before you begin, you need:
+ A user pool with an app client\. For more information, see [Getting Started with User Pools](getting-started-with-cognito-user-pools.md)\.
+ Set multi\-factor authentication \(MFA\) to **Optional** in the Amazon Cognito console to use the risk\-based adaptive authentication feature\. For more information, see [Adding Multi\-Factor Authentication \(MFA\) to a User Pool](user-pool-settings-mfa.md)\.
+ If you're using email for notifications, go to the [Amazon SES console](https://console.aws.amazon.com/ses/home) to configure and verify an email address or domain to use with your notification emails\. For more information about Amazon SES, see [Verifying Identities in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html)\.

## Configuring Advanced Security Features<a name="cognito-user-pool-configure-advanced-security"></a>

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

1. Choose **Customize** to customize adaptive authentication notifications with both HTML and plaintext email versions\. To learn more about email templates, see [Message Templates](cognito-user-pool-settings-message-templates.md)\.

1. Type any IP addresses that you want to **Always allow**, or **Always block**, regardless of the advanced security risk assessment\. Specify the IP address ranges in [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation) \(such as 192\.168\.100\.0/24\)\.

1. Choose **Save changes**\.