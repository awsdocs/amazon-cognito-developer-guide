# Email settings for Amazon Cognito user pools<a name="user-pool-email"></a>

Certain events in your user pool's client app can cause Amazon Cognito to email your users\. For example, if you configure your user pool to require email verification, Amazon Cognito sends an email when a user signs up for a new account in your app, or resets their password\. Depending on the action that initiates the email, the email will contain a verification code or a temporary password\.

To handle email delivery, you can use either of the following options:
+ [The default email functionality](#user-pool-email-default) that is built into the Amazon Cognito service\.
+ [Your Amazon SES configuration](#user-pool-email-developer)\.

These settings are reversible\. When needed, you can update your user pool to switch between them\.

## Default email functionality<a name="user-pool-email-default"></a>

Amazon Cognito can handle email deliveries for you by using the service's default email functionality\. When you use the default option, Amazon Cognito only allows a limited number of emails each day for your user pool\. For information on service limits, see [Quotas in Amazon Cognito](limits.md)\. For typical production environments, the default email limit is below the required delivery volume\. To enable a higher delivery volume, you can use your Amazon SES email configuration\.

With the default option, you can use either of the following email addresses as the FROM address:
+ The default email address, which is no\-reply@verificationemail\.com\.
+ A custom email address that you own\. Before you can use your own email address, you must verify it with Amazon SES\. You must also grant Amazon Cognito permission to use this address\.

## Amazon SES email configuration<a name="user-pool-email-developer"></a>

Your application might require a higher delivery volume than what is available with the default option\. To enable a larger delivery volume, use your Amazon SES configuration to allow your user pool to email your users\. Using your Amazon SES configuration also provides a greater ability to [monitor your email sending activity](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/monitor-sending-activity.html)\.

Before you can use your Amazon SES configuration, you must verify one or more email addresses, or a domain, with Amazon SES\. You use a verified email address, or an address from a verified domain, as the FROM email address that you assign to your user pool\. When Amazon Cognito sends email to a user, it uses your email address by calling Amazon SES for you\.

When you use your Amazon SES configuration, the email delivery limits for your user pool are the same limits that apply to your Amazon SES verified email address in your AWS account\.

### Amazon SES email configuration regions<a name="user-pool-email-developer-region-mapping"></a>

When determining the Amazon SES configuration Region to use, you must choose from the following Regions: `us-east-1`, `us-west-2`, or `eu-west-1`\.

For example, if your user pool is in a Region listed in the following table, use the following recommended Amazon SES configuration Region\. This can help you to determine which supported Amazon SES Region is geographically closest to your user pool setting\.


| Example Region | Example recommended Region | 
| --- | --- | 
| us\-east\-1  |  us\-east\-1  | 
|  ca\-central\-1  |  us\-east\-1  | 
|  us\-east\-2  |  us\-east\-1  | 
|  us\-west\-2  |  us\-west\-2  | 
|  ap\-northeast\-1  |  us\-west\-2  | 
|  ap\-northeast\-2  |  us\-west\-2  | 
|  ap\-south\-1  |  us\-west\-2  | 
|  ap\-southeast\-1  |  us\-west\-2  | 
|  ap\-southeast\-2  |  us\-west\-2  | 
|  eu\-west\-1  |  eu\-west\-1  | 
|  eu\-central\-1  |  eu\-west\-1  | 
|  eu\-west\-2  |  eu\-west\-1  | 

## Configuring email for your user pool<a name="user-pool-email-configure"></a>

Complete the following steps to configure the email settings for your user pool\. Depending on the settings that you use, you might need IAM permissions in Amazon SES, AWS Identity and Access Management \(IAM\), and Amazon Cognito\.

**Note**  
The resources that you create in these steps can't be shared across AWS accounts\. For example, you can't configure a user pool in one account, and then use it with an Amazon SES email address in a different account\. If you use Amazon Cognito in multiple accounts, remember to repeat these steps in each of them\.

### Step 1: Verify your email address or domain with Amazon SES<a name="user-pool-email-configure-verify-ses"></a>

Before you configure your user pool, you must verify one or more domains or email addresses with Amazon SES if you want to do either of the following:
+ Use your own email address as the FROM address
+ Use your Amazon SES configuration to handle email delivery

By verifying your email address or domain, you confirm that you own it, which helps prevent unauthorized use\.

For information on verifying an email address with Amazon SES, see [Verifying an Email Address](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-email-addresses-procedure.html) in the *Amazon Simple Email Service Developer Guide*\. For information on verifying a domain with Amazon SES, see [Verifying domains](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domains.html)\.

### Step 2: Move your account out of the Amazon SES sandbox<a name="user-pool-email-configure-sandbox"></a>

This step is only required if you are not using Amazon Cognito's default email functionality\.

When you first use Amazon SES, your AWS account is placed in the Amazon SES sandbox\. Amazon SES uses the sandbox to prevent fraud and abuse\. If you are using your Amazon SES configuration to handle email delivery, you must move your AWS account out of the sandbox before Amazon Cognito can email your users\.

In the sandbox, Amazon SES imposes restrictions on how many emails you can send and where you can send them\. You can send emails only to addresses and domains that you have verified with Amazon SES, or you can send them to Amazon SES mailbox simulator addresses\. While your AWS account remains in the sandbox, you should not use your Amazon SES configuration for applications that are in production\. In this situation, Amazon Cognito can't to send messages to your users' email addresses\.

To remove your AWS account from the sandbox, see [Moving out of the Amazon SES sandbox](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/request-production-access.html) in the *Amazon Simple Email Service Developer Guide\.*

### Step 3: Grant email permissions to Amazon Cognito<a name="user-pool-email-permissions"></a>

You might need to grant specific permissions to Amazon Cognito before it can email your users\. The permissions that you grant, and the process that you use to grant them, depend on whether you are using the default email functionality, or your Amazon SES configuration\.

#### To grant permissions to use the default email functionality<a name="user-pool-email-permissions-default"></a>

This step is only required if you are not using Amazon Cognito's default email functionality\.

If you configure your user pool to use Amazon Cognito's default email functionality, you can use either of the following addresses as the FROM address from which Amazon Cognito emails your users:
+ The default address
+ A custom address, which must be a verified email address, or an email address in a verified domain, in Amazon SES

If you use a custom address, Amazon Cognito needs additional permissions to email users from that address\. These permissions are granted by a *sending authorization policy* attached to the address or domain in Amazon SES\. If you use the Amazon Cognito console to add a custom address to your user pool, the policy is automatically attached to the Amazon SES verified email address\. However, if you configure your user pool outside of the console, such as using the AWS CLI or the Amazon Cognito API, you must attach the policy using the [Amazon SES console](https://console.aws.amazon.com/ses/) or the [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) API\.

**Note**  
You can only configure a FROM address in a verified domain using the AWS CLI or the Amazon Cognito API\.

A sending authorization policy allows or denies access based on the account resources that are using Amazon Cognito to invoke Amazon SES\. For more information about resource\-based policies, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_resource-based)\. You can also find example resource\-based policies in the [Amazon SES Developer Guide](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policy-examples.html)\.

**Example Sending authorization policy**  
The following example sending authorization policy grants Amazon Cognito a limited ability to use an Amazon SES verified identity\. Amazon Cognito can only send email messages when it does so on behalf of both the user pool in the `aws:SourceArn` condition and the account in the `aws:SourceAccount` condition\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "stmnt1234567891234",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "email.cognito-idp.amazonaws.com"
                ]
            },
            "Action": [
                "SES:SendEmail",
                "SES:SendRawEmail"
            ],
            "Resource": "<your SES identity ARN>",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceAccount": "<your account number>"
                },
                "ArnLike": {
                    "AWS:SourceArn": "<your identity pool ARN>"
                }
            }
        }
    ]
}
```
In this example, the "Sid" value is an arbitrary string that uniquely identifies the statement\.   
For more information about policy syntax, see [Amazon SES sending authorization policies](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policies.html) in the *Amazon Simple Email Service Developer Guide*\.  
For more examples, see [Amazon SES sending authorization policy examples](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policy-examples.html) in the *Amazon Simple Email Service Developer Guide*\.

**Example**  

#### To grant permissions to use your Amazon SES configuration<a name="user-pool-email-permissions-developer"></a>

If you configure your user pool to use your Amazon SES configuration, Amazon Cognito needs additional permissions to call Amazon SES on your behalf when it emails your users\. This authorization is granted with the IAM service\.

When you configure your user pool with this option, Amazon Cognito creates a *service\-linked role*, which is a type of IAM role, in your AWS account\. This role contains the permissions that allow Amazon Cognito to access Amazon SES and send email with your address\.

Before Amazon Cognito can create this role, the IAM permissions that you use to set up your user pool must include the `iam:CreateServiceLinkedRole` action\. For more information about updating permissions in IAM, see [Changing permissions for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

For more information about the service\-linked role that Amazon Cognito creates, see [Using service\-linked roles for Amazon Cognito](using-service-linked-roles.md)\.

### Step 4: Configure your user pool<a name="user-pool-email-configure"></a>

Complete the following steps if you want to configure your user pool with any of the following:
+ A custom FROM address that appears as the email sender
+ A custom REPLY\-TO address that receives the messages that your users send to your FROM address
+ Your Amazon SES configuration

You don't need to complete this procedure if you want to use the default Amazon Cognito email functionality and address\.

------
#### [ Original console ]

**To configure your user pool to use a custom email address**

1. Open the Amazon Cognito console at [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to configure\.

1. In the navigation menu on the left, choose **Message customizations**\.

1. If you want to use a custom FROM address, choose **Add custom FROM address** and complete the following steps:

   1. For **SES Region**, choose the Region that contains your verified email address\.

   1. For **Source ARN**, choose your email address\. You must use an email address this is verified with Amazon SES in the selected Region\.

   1. For **FROM email address**, choose your email address\. You can provide only your email address, or your email address and a friendly name in the format `Jane Doe <janedoe@example.com>`\.

1. Under **Do you want to send emails through your Amazon SES Configuration?**, choose **Yes \- Use Amazon SES** or **No \- Use Cognito \(Default\)**\.

   If you choose to use Amazon SES, Amazon Cognito will create a service\-linked role after you save your changes\.

1. If you want to use a custom REPLY\-TO address, choose **Add custom REPLY\-TO address**\. Then, enter the email address where you want to receive messages that your users send to your FROM address\.

1. When you finish setting your email account options, choose **Save changes**\.

------
#### [ New console ]

**To configure your user pool to use a custom email address**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Messaging** tab, locate **Email configuration**, choose **Edit**\.

1. On the **Edit email configuration** page, select **Send email from Amazon SES** or **Send email with Amazon Cognito**\. You can customize the **SES Region**, **Configuration Set**, and **FROM sender name** only when you choose **Send email from Amazon SES**\.

1. To use a custom FROM address, complete the following steps:

   1. Under **SES Region**, choose the Region that contains your verified email address\.

   1. Under **FROM email address**, choose your email address\. You must use an email address that is verified with Amazon SES\.

   1. \(Optional\) Under **Configuration set**, choose a configuration set for use with Amazon SES\. This will create a service\-linked role after you save your changes\.

   1. \(Optional\) Under **FROM sender address**, enter an email address\. You can provide only an email address, or an email address and a friendly name in the format `Jane Doe <janedoe@example.com>`\.

   1. \(Optional\) Under **REPLY\-TO email address**, enter the email address where you want to receive messages that your users send to your FROM address\.

1. Choose **Save changes**\.

------

**Related Topics**
+ [Customizing email verification messages](cognito-user-pool-settings-email-verification-message-customization.md)
+ [Customizing user invitation messages](cognito-user-pool-settings-user-invitation-message-customization.md)