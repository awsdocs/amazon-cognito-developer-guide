# Creating user accounts as administrator<a name="how-to-create-user-accounts"></a>

After you create your user pool, you can create users using the AWS Management Console, as well as the AWS Command Line Interface or the Amazon Cognito API\. You can create a profile for a new user in a user pool and send a welcome message with sign\-up instructions to the user via SMS or email\.

Developers and administrators can perform the following tasks:
+ Create a new user profile by using the AWS Management Console or by calling the `AdminCreateUser` API\.
+ Specify the temporary password or allow Amazon Cognito to automatically generate one\.
+ Specify whether provided email addresses and phone numbers are marked as verified for new users\.
+ Specify custom SMS and email invitation messages for new users via the AWS Management Console or a Custom Message Lambda trigger\. For more information, see [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.
+ Specify whether invitation messages are sent via SMS, email, or both\.
+ Resend the welcome message to an existing user by calling the `AdminCreateUser` API, specifying `RESEND` for the `MessageAction` parameter\.
**Note**  
This action cannot currently be performed using the AWS Management Console\.
+ Suppress the sending of the invitation message when the user is created\.
+ Specify an expiration time limit for the user account \(up to 90 days\)\.
+ Allow users to sign themselves up or require that new users only be added by the administrator\.

## Authentication flow for users created by administrators or developers<a name="authentication-flow-for-create-user"></a>

The authentication flow for these users includes the extra step to submit the new password and provide any missing values for required attributes\. The steps are outlined next; steps 5, 6, and 7 are specific to these users\.

1. The user starts to sign in for the first time by submitting the user name and password provided to him or her\.

1. The SDK calls `InitiateAuth(Username, USER_SRP_AUTH)`\.

1. Amazon Cognito returns the `PASSWORD_VERIFIER` challenge with Salt & Secret block\.

1. The SDK performs the SRP calculations and calls `RespondToAuthChallenge(Username, <SRP variables>, PASSWORD_VERIFIER)`\.

1. Amazon Cognito returns the `NEW_PASSWORD_REQUIRED` challenge\. The body of this challenge includes the user's current attributes, and any required attributes in your user pool that don't currently have a value in the user's profile\. For more information, see [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html)\.

1. The user is prompted and enters a new password and any missing values for required attributes\.

1. The SDK calls `RespondToAuthChallenge(Username, <New password>, <User attributes>)`\.

1. If the user requires a second factor for MFA, Amazon Cognito returns the SMS\_MFA challenge and the code is submitted\.

1. After the user has successfully changed his or her password and optionally provided attributed values or completed MFA, the user is signed in and tokens are issued\.

When the user has satisfied all challenges, the Amazon Cognito service marks the user as confirmed and issues ID, access, and refresh tokens for the user\. For more information, see [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

## Creating a new user in the AWS Management Console<a name="creating-a-new-user-using-the-console"></a>

You can set user password requirements, configure the invitation and verification messages sent to users, and add new users with the Amazon Cognito console\.

### Set a password policy and enable self\-registration<a name="set-user-password-policy"></a>

------
#### [ Original console ]

The **Policies** tab has these related settings:
+ Specify the required password strength\.  
![\[Specify the required password strength.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify the required password strength.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify the required password strength.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)
+ Specify whether to allow users to sign themselves up\. This option is set by default\.  
![\[Specify whether to allow users to sign themselves up. This option is selected by default.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify whether to allow users to sign themselves up. This option is selected by default.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify whether to allow users to sign themselves up. This option is selected by default.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)
+ Specify user account expiration time limit \(in days\) for new accounts\. The default setting is 7 days, measured from the time when the user account is created\. The maximum setting is 90 days\. After the account expires, the user cannot log in to the account until the administrator updates the user's profile\.
**Note**  
Once the user has logged in, the account never expires\.  
![\[Specify user account expiration time limit (in days). The default setting is 10 days.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify user account expiration time limit (in days). The default setting is 10 days.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify user account expiration time limit (in days). The default setting is 10 days.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ New console ]

You can configure settings for minimum password complexity and whether users can sign up using public APIs in your user pool\.

**Configure a password policy**

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab and locate **Password policy**\. Choose **Edit**\.

1. Choose a **Password policy mode** of **Custom**\.

1. Choose a **Password minimum length**\. For limits to the password length requirement, see [User pools resource quotas](https://docs.aws.amazon.com/cognito/latest/developerguide/limits.html#limits-hard)\.

1. Choose a **Password complexity** requirement\.

1. Choose how long password set by administrators should be valid for\.

1. Choose **Save changes**\.

**Allow self\-service sign\-up**

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-up experience** tab and locate **Self\-service sign\-up**\. Select **Edit**\.

1. Choose whether to **Enable self\-registration**\. Self\-registration is typically used with public app clients that need to register new users in your user pool without distributing a client secret or AWS Identity and Access Management \(IAM\) API credentials\.
**Disabling self\-registration**  
If you do not enable self\-registration, new users must be created by administrative API actions using IAM API credentials or by sign\-in with federated providers\.

1. Choose **Save changes**\.

------

### Customize email and SMS messages<a name="creating-a-new-user-customize-messages"></a>

------
#### [ Original console ]

The **Message Customizations** tab includes templates for specifying custom email verification and user invitation messages\.

For email verification or user invitation messages, the maximum length for the message is 2048 UTF\-8 characters, including the verification code or temporary password\. For SMS verification or user invitation messages, the maximum length is 140 UTF\-8 characters, including the verification code or temporary password\.

Verification codes are valid for 24 hours\.

![\[Customize the email verification sent to users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the email verification sent to users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the email verification sent to users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

![\[Customize the invitation message sent to new users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the invitation message sent to new users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the invitation message sent to new users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ New console ]

**Customize user messages**

You can customize the messages that Amazon Cognito sends to your users when you invite them to sign in, they sign up for a user account, or they sign in and are prompted for multi\-factor authentication \(MFA\)\.
**Note**  
An **Invitation message** is sent when you create a user in your user pool and invite them to sign in\. Amazon Cognito sends initial sign\-in information to the user's email address or phone number\.  
A **Verification message** is sent when a user signs up for a user account in your user pool\. Amazon Cognito sends a code to the user\. When the user provides the code to Amazon Cognito, they verify their contact information and confirm their account for sign\-in\. Verification codes are valid for 24 hours\.  
An **MFA message** is sent when you enable SMS MFA in your user pool, and a user that has configured SMS MFA signs in and is prompted for MFA\.

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Messaging** tab and locate **Message templates**\. Select **Verification messages**, **Invitation messages**, or **MFA messages** and choose **Edit**\.

1. Customize the messages for the chosen message type\.
**Note**  
All variables in message templates must be included when you customize the message\. If the variable, for example **\{\#\#\#\#\}**, is not included, your user will have insufficient information to complete the message action\.  
For more information, see [Message templates](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-message-templates.html)\.

1. 

   1. **Verification messages**

      1. Choose a **Verification type** for **Email** messages\. A **Code** verification sends a numeric code that the user must enter\. A **Link** verification sends a link the user can click to verify their contact information\. The text in the variable for a **Link** message is displayed as hyperlink text\. For example, a message template using the variable \{\#\#Click here\#\#\} is displayed as [Click here]() in the email message\.

      1. Enter an **Email subject** for **Email** messages\.

      1. Enter a custom **Email message** template for **Email** messages\. You can customize this template with HTML\.

      1. Enter a custom **SMS message** template for **SMS** messages\.

      1. Choose **Save changes**\.

   1. **Invitation messages**

      1. Enter an **Email subject** for **Email** messages\.

      1. Enter a custom **Email message** template for **Email** messages\. You can customize this template with HTML\.

      1. Enter a custom **SMS message** template for **SMS** messages\.

      1. Choose **Save changes**\.

   1. **MFA messages**

      1. Enter a custom **SMS message** template for **SMS** messages\.

      1. Choose **Save changes**\.

------

### Create a user<a name="creating-a-new-user-using-the-users-tab"></a>

------
#### [ Original console ]

The **Users** tab in the **Users and groups** tab has a **Create user** button\.

![\[Create a new user in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create a new user in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create a new user in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

When you choose **Create user**, a **Create user** dialog appears, where you can enter information about the new user\. Only the **Username** field is required\. 

**Note**  
For user accounts that you create by using the **Create user** form in the AWS Management Console, only the attributes shown in the form can be set in the AWS Management Console\. Other attributes must be set by using the AWS Command Line Interface or the Amazon Cognito API, even if you have marked them as required attributes\.

![\[Create user form in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create user form in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create user form in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ New console ]

**Create a user**

You can create new users for your user pool from the Amazon Cognito console\. Typically, users can sign in after they set a password\. To sign in with an email address, a user must verify the `email` attribute\. To sign in with a phone number, the user must verify the `phone_number` attribute\. To confirm accounts as an administrator, you can also use the AWS CLI or API, or create user profiles with a federated identity provider\. For more information, see the [Amazon Cognito API Reference](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/)\.

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Users** tab, and choose **Create a user**\.

1. Review the **User pool sign\-in and security requirements** for guidance on password requirements, available account recovery methods, and alias attributes for your user pool\.

1. Choose how you want to send an **Invitation message**\. Choose SMS message, email message, or both\.
**Note**  
Before you can send invitation messages, configure a sender and an AWS Region with Amazon Simple Notification Service and Amazon Simple Email Service in the **Messaging** tab of your user pool \. Recipient message and data rates apply\. Amazon SES bills you for email messages separately, and Amazon SNS bills you for SMS messages separately\.

1. Choose a **Username** for the new user\.

1. Choose if you want to **Create a password** or have Amazon Cognito **Generate a password** for the user\. Any temporary password must adhere to the user pool password policy\.

1. Choose **Create**\.

1. Choose the **Users** tab, and choose the **User name** entry for the user\. Add and edit **User attributes** and **Group memberships**\. Review **User event history**\.

------