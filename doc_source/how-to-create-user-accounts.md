# Creating User Accounts as Administrator<a name="how-to-create-user-accounts"></a>

After you create your user pool, you can create users using the AWS Management Console, as well as the AWS Command Line Interface or the Amazon Cognito API\. You can create a profile for a new user in a user pool and send a welcome message with sign\-up instructions to the user via SMS or email\.

Developers and administrators can perform the following tasks:
+ Create a new user profile by using the AWS Management Console or by calling the `AdminCreateUser` API\.
+ Specify the temporary password or allow Amazon Cognito to automatically generate one\.
+ Specify whether provided email addresses and phone numbers are marked as verified for new users\.
+ Specify custom SMS and email invitation messages for new users via the AWS Management Console or a Custom Message Lambda trigger\. For more information, see [Customizing User Pool Workflows with Lambda Triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.
+ Specify whether invitation messages are sent via SMS, email, or both\.
+ Resend the welcome message to an existing user by calling the `AdminCreateUser` API, specifying `RESEND` for the `MessageAction` parameter\.
**Note**  
This action cannot currently be performed using the AWS Management Console\.
+ Suppress the sending of the invitation message when the user is created\.
+ Specify an expiration time limit for the user account \(up to 90 days\)\.
+ Allow users to sign themselves up or require that new users only be added by the administrator\.

## Authentication Flow for Users Created by Administrators or Developers<a name="authentication-flow-for-create-user"></a>

The authentication flow for these users includes the extra step to submit the new password and provide any missing values for required attributes\. The steps are outlined next; steps 5, 6, and 7 are specific to these users\.

1. The user starts to sign in for the first time by submitting the username and password provided to him or her\.

1. The SDK calls `InitiateAuth(Username, USER_SRP_AUTH)`\.

1. Amazon Cognito returns the `PASSWORD_VERIFIER` challenge with Salt & Secret block\.

1. The SDK performs the SRP calculations and calls `RespondToAuthChallenge(Username, <SRP variables>, PASSWORD_VERIFIER)`\.

1. Amazon Cognito returns the `NEW_PASSWORD_REQUIRED` challenge along with the current and required attributes\.

1. The user is prompted and enters a new password and any missing values for required attributes\.

1. The SDK calls `RespondToAuthChallenge(Username, <New password>, <User attributes>)`\.

1. If the user requires a second factor for MFA, Amazon Cognito returns the SMS\_MFA challenge and the code is submitted\.

1. After the user has successfully changed his or her password and optionally provided attributed values or completed MFA, the user is signed in and tokens are issued\.

When the user has satisfied all challenges, the Amazon Cognito service marks the user as confirmed and issues ID, access, and refresh tokens for the user\. For more information, see [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

## Creating a New User in the AWS Management Console<a name="creating-a-new-user-using-the-console"></a>

The Amazon Cognito console for managing user pools has been updated to support this feature, as shown next\.

### Policies Tab<a name="creating-a-new-user-using-the-policies-tab"></a>

The **Policies** tab has these related settings:
+ Specify the required password strength\.  
![\[Specify the required password strength.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify the required password strength.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify the required password strength.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)
+ Specify whether to allow users to sign themselves up\. This option is set by default\.  
![\[Specify whether to allow users to sign themselves up. This option is selected by default.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify whether to allow users to sign themselves up. This option is selected by default.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify whether to allow users to sign themselves up. This option is selected by default.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)
+ Specify user account expiration time limit \(in days\) for new accounts\. The default setting is 7 days, measured from the time when the user account is created\. The maximum setting is 90 days\. After the account expires, the user cannot log in to the account until the administrator updates the user's profile\.
**Note**  
Once the user has logged in, the account never expires\.  
![\[Specify user account expiration time limit (in days). The default setting is 10 days.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify user account expiration time limit (in days). The default setting is 10 days.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Specify user account expiration time limit (in days). The default setting is 10 days.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Message Customizations Tab<a name="creating-a-new-user-using-the-message-customization-tab"></a>

The **Message Customizations** tab includes templates for specifying custom email verification messages and custom user invitation messages\.

For email \(verification messages or user invitation messages\), the maximum length for the message is 2048 UTF\-8 characters, including the verification code or temporary password\. For SMS, the maximum length is 140 UTF\-8 characters, including the verification code or temporary password\.

Verification codes are valid for 24 hours\.

![\[Customize the email verification sent to users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the email verification sent to users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the email verification sent to users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

![\[Customize the invitation message sent to new users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the invitation message sent to new users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Customize the invitation message sent to new users.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Users Tab<a name="creating-a-new-user-using-the-users-tab"></a>

The **Users** tab in the **Users and groups** tab has a **Create user** button\.

![\[Create a new user in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create a new user in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create a new user in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

When you choose **Create user**, a **Create user** form appears, which you can use to enter information about the new user\. Only the **Username** field is required\. 

**Note**  
For user accounts that you create by using the **Create user** form in the AWS Management Console, only the attributes shown in the form can be set in the AWS Management Console\. Other attributes must be set by using the AWS Command Line Interface or the Amazon Cognito API, even if you have marked them as required attributes\.

![\[Create user form in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create user form in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Create user form in the Users tab.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)