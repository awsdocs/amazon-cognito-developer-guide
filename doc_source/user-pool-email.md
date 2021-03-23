# Email Settings for Amazon Cognito User Pools<a name="user-pool-email"></a>

Certain events in the client app for your user pool might cause Amazon Cognito to email your users\. For example, if you configure your user pool to require email verification, Amazon Cognito sends an email when a user signs up for a new account in your app or resets their password\. Depending on the action that initiates the email, the email contains a verification code or a temporary password\.

To handle email delivery, you can use either of the following options:
+ [The default email functionality](#user-pool-email-default) that is built into the Amazon Cognito service\.
+ [Your Amazon SES configuration](#user-pool-email-developer)\.

These settings are reversible\. When needed, you can update your user pool to switch between them\.

## Default Email Functionality<a name="user-pool-email-default"></a>

You can allow Amazon Cognito to handle email deliveries for you by using the default email functionality that comes with the service\. When you use the default option, Amazon Cognito allows only a limited number of emails each day for your user pool\. For specific limits information, see [Quotas in Amazon Cognito](limits.md)\. For typical production environments, the default email limit is below the required delivery volume\. To enable a higher delivery volume, you must use your Amazon SES email configuration\.

With the default option, you can use either of the following email addresses as the FROM address:
+ The default email address, which is no\-reply@verificationemail\.com\.
+ A custom email address that you own\. Before you can use your own email address, you must verify it with Amazon SES, and you must grant Amazon Cognito permission to use it\.

## Amazon SES Email Configuration<a name="user-pool-email-developer"></a>

Your application might require a higher delivery volume than what is available with the default option\. To enable a higher delivery volume, configure your user pool to email your users by using your Amazon SES configuration\. Using your Amazon SES configuration also provides a greater ability to [monitor your email sending activity](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/monitor-sending-activity.html)\.

Before you can use your Amazon SES configuration, you must verify one or more email addresses with Amazon SES\. You use a verified email addresses as the FROM email address that you assign to your user pool\. Then, when Amazon Cognito sends an email, it uses your email address by calling Amazon SES on your behalf\.

When you use your Amazon SES configuration, the email delivery limits for your user pool are the same limits that apply to your Amazon SES verified email address in your AWS account\.

**Note**  
Available regions for Amazon Cognito are us\-east\-1, us\-east\-2, us\-west\-2, eu\-west\-1, eu\-west\-2, eu\-central\-1, ap\-northeast\-1, ap\-northeast\-2, ap\-south\-1, ap\-southeast\-1, ap\-southeast\-2, and ca\-central\-1\. Available Amazon SES regions are: us\-east\-1, us\-west\-2, eu\-west\-1\. 

## Configuring Email for Your User Pool<a name="user-pool-email-configure"></a>

Complete the following steps to configure the email settings for your user pool\. Depending on settings that you want to use, you might need to complete steps with Amazon SES, AWS Identity and Access Management \(IAM\), and Amazon Cognito\.

**Note**  
The resources that you create in these steps can't be shared across AWS accounts\. For example, you can't configure a user pool in one account with an Amazon SES email address that is in a different account\. Therefore, if you use Amazon Cognito in multiple accounts, remember to repeat these steps in each of them\.

### Step 1: Verify Your Email Address with Amazon SES<a name="user-pool-email-configure-verify-ses"></a>

Before you configure your user pool, you must verify one or more email addresses with Amazon SES if you want to do either of the following:
+ Use your own email address as the FROM address
+ Use your Amazon SES configuration to handle email delivery

By verifying your email address, you confirm that you own it, which helps prevent unauthorized use\.

For the steps to this procedure, see [Verifying an Email Address](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses-procedure.html) in the *Amazon Simple Email Service Developer Guide*\.

### Step 2: Move Your Account Out of the Amazon SES Sandbox<a name="user-pool-email-configure-sandbox"></a>

When you first begin using Amazon SES, your AWS account is placed in the Amazon SES sandbox\. Amazon SES uses the sandbox to prevent fraud and abuse\. If you are using your Amazon SES configuration to handle email delivery, you must move your AWS account out of the sandbox before Amazon Cognito can email your users\.

You can skip this step if you are using the default Amazon Cognito email functionality\.

In the sandbox, Amazon SES imposes restrictions on how many emails you can send and where you can send them\. You can send emails only to addresses and domains that you have verified with Amazon SES, or you can send them to Amazon SES mailbox simulator addresses\. While your AWS account remains in the sandbox, do not use your Amazon SES configuration for applications that are in production\. In this situation, Amazon Cognito would be unable to send messages to your users' email addresses\.

For the steps to move out of the sandbox, see [Moving Out of the Amazon SES Sandbox](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html) in the *Amazon Simple Email Service Developer Guide\.*

### Step 3: Grant Email Permissions to Amazon Cognito<a name="user-pool-email-permissions"></a>

You might need to grant specific permissions to Amazon Cognito before it can email your users\. The permissions that you grant, and the process that you use to grant them, depend on whether you are using the default email functionality or your Amazon SES configuration\.

#### To Grant Permissions to Use the Default Email Functionality<a name="user-pool-email-permissions-default"></a>

If you configure your user pool to use the default email functionality, you use either of the following addresses as the FROM address from which Amazon Cognito emails your users:
+ The default address
+ A custom address, which must be a verified address in Amazon SES

If you are using the default email address, Amazon Cognito does not need additional permissions, and you can skip this step\. 

If you are using a custom address, Amazon Cognito needs additional permissions so that it can use this address to email your users\. These permissions are granted by a *sending authorization policy*, which gets attached to the address in Amazon SES\. If you use the Amazon Cognito console to add your custom address to your user pool, it automatically attaches the policy for you\. But, if you configure your user pool outside of the console, for example by using the AWS CLI or Amazon Cognito API, you must attach the policy yourself\.

For more information, see [Using Sending Authorization with Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization.html) in the *Amazon Simple Email Service Developer Guide*\. 

**Example Sending Authorization Policy**  
The following example is a sending authorization policy that allows Amazon Cognito to send email by using an Amazon SES verified email address\.  

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "stmnt1234567891234",
            "Effect": "Allow",
            "Principal": {
                "Service": "cognito-idp.amazonaws.com"
            },
            "Action": [
                "ses:SendEmail",
                "ses:SendRawEmail"
            ],
            "Resource": "<your SES identity ARN>"
        }
    ]
}
```
In this example, the "Sid" value is an arbitrary string that uniquely identifies the statement\.   
For more information about policy syntax, see [Amazon SES Sending Authorization Policies](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policies.html) in the *Amazon Simple Email Service Developer Guide*\.  
For more examples, see [Amazon SES Sending Authorization Policy Examples](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policy-examples.html) in the *Amazon Simple Email Service Developer Guide*\.

#### To Grant Permissions to Use Your Amazon SES Configuration<a name="user-pool-email-permissions-developer"></a>

If you configure your user pool to use your Amazon SES configuration, Amazon Cognito needs additional permissions to call Amazon SES on your behalf when it emails your users\. This authorization is granted with the IAM service\.

When you configure your user pool with this option, Amazon Cognito creates a *service\-linked role*, which is a type of IAM role, in your AWS account\. This role contains the permissions that allow Amazon Cognito to access Amazon SES and send email with your address\.

Before Amazon Cognito can create this role, the IAM permissions that you use to set up your user pool must include the `iam:CreateServiceLinkedRole` action\. For more information about updating permissions in IAM, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

For more information about the service\-linked role that Amazon Cognito creates, see [Using Service\-Linked Roles for Amazon Cognito](using-service-linked-roles.md)\.

### Step 4: Configure Your User Pool<a name="user-pool-email-configure"></a>

Complete the following steps if you want to configure your user pool with any of the following:
+ A custom FROM address that appears as the email sender
+ A custom REPLY\-TO address that receives the messages that your users send to your FROM address
+ Your Amazon SES configuration

You don't need to complete this procedure if you want to use the default Amazon Cognito email functionality and address\.

**To configure your email address**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to configure\.

1. In the navigation menu on the left, choose **Message customizations**\.

1. If you want to use a custom FROM address, choose **Add custom FROM address** and do the following:

   1. For **SES region**, choose the region that contains your verified email address\.

   1. For **Source ARN**, choose your email address\. The Amazon Cognito console allows you to choose only those email addresses that you have verified with Amazon SES in the selected region\.

   1. For **FROM email address**, choose your email address\. You can provide your email address or your email address with your name\.

1. Under **Do you want to send emails through your Amazon SES Configuration?**, choose **Yes \- Use Amazon SES** or **No \- Use Cognito \(Default\)**\.

   If you choose to use a Amazon SES, Amazon Cognito will create a service\-linked role after you save your changes\.

1. If you want to use a custom REPLY\-TO address, choose **Add custom REPLY\-TO address**\. Then, specify the email address where you want to receive that messages that your users send to your FROM address\.

1. When you finish setting your email account options, choose **Save changes**\.

The **Message customizations** page also provides the options to [customize your verification messages](cognito-user-pool-settings-email-verification-message-customization.md) and [customize your invitation messages](cognito-user-pool-settings-user-invitation-message-customization.md)\.