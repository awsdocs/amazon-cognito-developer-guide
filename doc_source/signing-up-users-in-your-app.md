# Signing up and confirming user accounts<a name="signing-up-users-in-your-app"></a>

User accounts are added to your user pool in one of the following ways:
+ The user signs up in your user pool's client app\. This can be a mobile or web app\.
+ You can import the user's account into your user pool\. For more information, see [Importing users into user pools from a CSV file](cognito-user-pools-using-import-tool.md)\.
+ You can create the user's account in your user pool and invite the user to sign in\. For more information, see [Creating user accounts as administrator](how-to-create-user-accounts.md)\.

Users who sign themselves up need to be confirmed before they can sign in\. Imported and created users are already confirmed, but they must create their password the first time they sign in\. The following sections explain the confirmation process and email and phone verification\.

## Overview of user account confirmation<a name="signup-confirmation-verification-overview"></a>

The following diagram illustrates the confirmation process:

![\[When users enter the confirmation code, they automatically verify email or phone.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[When users enter the confirmation code, they automatically verify email or phone.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[When users enter the confirmation code, they automatically verify email or phone.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

A user account can be in any of the following states:

**Registered \(Unconfirmed\)**  
The user has successfully signed up, but cannot sign in until the user account is confirmed\. The user is enabled but not confirmed in this state\.  
New users who sign themselves up start in this state\.

**Confirmed**  
The user account is confirmed and the user can sign in\. When a user enters a code or follows an email link to confirm their user account, that email or phone number is automatically verified\. The code or link is valid for 24 hours\.  
If the user account was confirmed by the administrator or a pre sign\-up Lambda trigger, there might not be a verified email or phone number associated with the account\.

**Password Reset Required**  
The user account is confirmed, but the user must request a code and reset their password before they can sign in\.  
User accounts that are imported by an administrator or developer start in this state\.

**Force Change Password**  
The user account is confirmed and the user can sign in using a temporary password, but on first sign\-in, the user must changetheir password to a new value before doing anything else\.  
User accounts that are created by an administrator or developer start in this state\.

**Disabled**  
Before you can delete a user account, you must disable sign\-in access for that user\.

## Verifying contact information at sign\-up<a name="allowing-users-to-sign-up-and-confirm-themselves"></a>

When new users sign up in your app, you probably want them to provide at least one contact method\. For example, with your users' contact information, you might:
+ Send a temporary password when a user chooses to reset their password\.
+ Notify users when their personal or financial information is updated\.
+ Send promotional messages, such as special offers or discounts\.
+ Send account summaries or billing reminders\.

For use cases like these, it's important that you send your messages to a verified destination\. Otherwise, you might send your messages to an invalid email address or phone number that was typed incorrectly\. Or worse, you might send sensitive information to bad actors who pose as your users\.

To help ensure that you send messages only to the right individuals, configure your Amazon Cognito user pool so that users must provide the following when they sign up:

1. An email address or phone number\.

1. A verification code that Amazon Cognito sends to that email address or phone number\.

By providing the verification code, a user proves that they have access to the mailbox or phone that received the code\. After the user provides the code, Amazon Cognito updates the information about the user in your user pool by:
+ Setting the user's status to `CONFIRMED`\.
+ Updating the user's attributes to indicate that the email address or phone number is verified\.

To view this information, you can use the Amazon Cognito console\. Or, you can use the `AdminGetUser` API action, the `admin-get-user` command with the AWS CLI, or a corresponding action in one of the AWS SDKs\.

If a user has a verified contact method, Amazon Cognito automatically sends a message to the user when the user requests a password reset\.

### To configure your user pool to require email or phone verification<a name="verification-configure"></a>

When you verify your users' email addresses and phone numbers, you ensure that you can contact your users\. Complete the following steps in the AWS Management Console to configure your user pool to require that your users confirm their email addresses or phone numbers\.

**Note**  
If you don't yet have a user pool in your account, see [Getting started with user pools](getting-started-with-cognito-user-pools.md)\.

------
#### [ Original console ]

**To configure your user pool**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to configure\.

1. In the navigation menu on the left, choose **MFA and verifications**\.

1. Under **Which attributes do you want to verify?**, choose one of the following options:  
![\[The user pool configuration options that require users to verify an email address or phone number to sign up in your app.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/images/amazon-cognito-user-pool-email-or-phone-verification.png)  
**Email**  
If you choose this option, Amazon Cognito emails a verification code when the user signs up\. Choose this option if you typically communicate with your users through email\. For example, you will want to use verified email addresses if you send billing statements, order summaries, or special offers\.  
**Phone number**  
If you choose this option, Amazon Cognito sends a verification code through SMS message when the user signs up\. Choose this option if you typically communicate with your users through SMS messages\. For example, you will want to use verified phone numbers if you send delivery notifications, appointment confirmations, or alerts\.   
**Email or phone number**  
Choose this option if you don't require all users to have the same verified contact method\. In this case, the sign\-up page in your app could ask users to verify only their preferred contact method\. When Amazon Cognito sends a verification code, it sends the code to the contact method provided in the `SignUp` request from your app\. If a user provides both an email address and a phone number, and your app provides both contact methods in the `SignUp` request, Amazon Cognito sends a verification code only to the phone number\.  
If you require users to verify both an email address and a phone number, choose this option\. Amazon Cognito verifies one contact method when the user signs up, and your app must verify the other contact method after the user signs in\. For more information, see [If you require users to confirm both email addresses and phone numbers](#verification-email-plus-phone)\.  
**None**  
If you choose this option, Amazon Cognito doesn't send verification codes when users sign up\. Choose this option if you are using a custom authentication flow that verifies at least one contact method without using verification codes from Amazon Cognito\. For example, you might use a pre sign\-up Lambda trigger that automatically verifies email addresses that belong to a specific domain\.  
If you don't verify your users' contact information, they may be unable to use your app\. Remember that users require verified contact information to:  
   + **Reset their passwords** — When a user chooses an option in your app that calls the `ForgotPassword` API action, Amazon Cognito sends a temporary password to the user's email address or phone number\. Amazon Cognito sends this password only if the user has at least one verified contact method\.
   + **Sign in by using an email address or phone number as an alias** — If you configure your user pool to allow these aliases, then a user can sign in with an alias only if the alias is verified\. For more information, see [Aliases](user-pool-settings-attributes.md#user-pool-settings-aliases)\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To configure your user pool**

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. From the navigation pane, choose **User Pools**\. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-up experience** tab and locate **Attribute verification and user account confirmation**\. Choose **Edit**\.

1. Choose whether you will activate **Cognito\-assisted verification and confirmation** to have Amazon Cognito send messages to the user contact attributes you choose when a user signs up, or you create a user profile\. The messages that Amazon Cognito sends provide users with a code or link that, after they have confirmed they received it, verifies the attribute and confirms the user profile for sign\-in\. 
**Note**  
You can also disable **Cognito\-assisted verification and confirmation** and use authenticated API actions or Lambda triggers to verify attributes and confirm users\.  
If you choose this option, Amazon Cognito doesn't send verification codes when users sign up\. Choose this option if you are using a custom authentication flow that verifies at least one contact method without using verification codes from Amazon Cognito\. For example, you might use a pre sign\-up Lambda trigger that automatically verifies email addresses that belong to a specific domain\.  
If you don't verify your users' contact information, they may be unable to use your app\. Remember that users require verified contact information to:  
**Reset their passwords** — When a user chooses an option in your app that calls the `ForgotPassword` API action, Amazon Cognito sends a temporary password to the user's email address or phone number\. Amazon Cognito sends this password only if the user has at least one verified contact method\.
**Sign in by using an email address or phone number as an alias** — If you configure your user pool to allow these aliases, then a user can sign in with an alias only if the alias is verified\. For more information, see [Aliases](user-pool-settings-attributes.md#user-pool-settings-aliases)\.

1. Choose your **Attributes to verify**:  
**Send SMS message, verify phone number**  
Amazon Cognito sends a verification code in an SMS message when the user signs up\. Choose this option if you typically communicate with your users through SMS messages\. For example, you will want to use verified phone numbers if you send delivery notifications, appointment confirmations, or alerts\. User phone numbers will be the verified attribute when accounts are confirmed; you must take additional action to verify and communicate with user email addresses\.  
**Send email message, verify email address**  
Amazon Cognito sends a verification code through an email message when the user signs up\. Choose this option if you typically communicate with your users through email\. For example, you will want to use verified email addresses if you send billing statements, order summaries, or special offers\. User email addresses will be the verified attribute when accounts are confirmed; you must take additional action to verify and communicate with user phone numbers\.  
**Send SMS message if phone number is available, otherwise send email message**  
Choose this option if you don't require all users to have the same verified contact method\. In this case, the sign\-up page in your app could ask users to verify only their preferred contact method\. When Amazon Cognito sends a verification code, it sends the code to the contact method provided in the `SignUp` request from your app\. If a user provides both an email address and a phone number, and your app provides both contact methods in the `SignUp` request, Amazon Cognito sends a verification code only to the phone number\.  
If you require users to verify both an email address and a phone number, choose this option\. Amazon Cognito verifies one contact method when the user signs up, and your app must verify the other contact method after the user signs in\. For more information, see [If you require users to confirm both email addresses and phone numbers](#verification-email-plus-phone)\.

1. Choose **Save changes**\.

------

### Authentication flow with email or phone verification<a name="verification-flow"></a>

If your user pool requires users to verify their contact information, your app must facilitate the following flow when a user signs up:

1. A user signs up in your app by entering a username, phone number and/or email address, and possibly other attributes\.

1. The Amazon Cognito service receives the sign\-up request from the app\. After verifying that the request contains all attributes required for sign\-up, the service completes the sign\-up process and sends a confirmation code to the user's phone \(in an SMS message\) or email\. The code is valid for 24 hours

1. The service returns to the app that sign\-up is complete and that the user account is pending confirmation\. The response contains information about where the confirmation code was sent\. At this point the user's account is in an unconfirmed state, and the user's email address and phone number are unverified\.

1. The app can now prompt the user to enter the confirmation code\. It is not necessary for the user to enter the code immediately\. However, the user will not be able to sign in until after they enter the confirmation code\.

1. The user enters the confirmation code in the app\.

1. The app calls [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html) to send the code to the Amazon Cognito service, which verifies the code and, if the code is correct, sets the user's account to the confirmed state\. After successfully confirming the user account, the Amazon Cognito service automatically marks the attribute that was used to confirm \(email address or phone number\) as verified\. Unless the value of this attribute is changed, the user will not have to verify it again\.

1. At this point the user's account is in a confirmed state, and the user can sign in\.

### If you require users to confirm both email addresses and phone numbers<a name="verification-email-plus-phone"></a>

Amazon Cognito verifies only one contact method when a user signs up\. In cases where Amazon Cognito must choose between verifying an email address or phone number, it chooses to verify the phone number by sending a verification code through SMS message\. For example, if you configure your user pool to allow users to verify either email addresses or phone numbers, and if your app provides both of these attributes upon sign\-up, Amazon Cognito verifies only the phone number\. After a user verifies his or her phone number, Amazon Cognito sets the user's status to `CONFIRMED`, and the user is allowed to sign in to your app\.

After the user signs in, your app can provide the option to verify the contact method that wasn't verified during sign\-up\. To verify this second method, your app calls the `VerifyUserAttribute` API action\. Note that this action requires an `AccessToken` parameter, and Amazon Cognito only provides access tokens for authenticated users\. Therefore, you can verify the second contact method only after the user signs in\.

If you require your users to verify both email addresses and phone numbers, do the following:

1. Configure your user pool to allow users to verify email address or phone numbers\.

1. In the sign\-up flow for your app, require users to provide both an email address and a phone number\. Call the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html) API action, and provide the email address and phone number for the `UserAttributes` parameter\. At this point, Amazon Cognito sends a verification code to the user's phone\.

1. In your app interface, present a confirmation page where the user enters the verification code\. Confirm the user by calling the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html) API action\. At this point, the user's status is `CONFIRMED`, and the user's phone number is verified, but the email address is not verified\.

1. Present the sign\-in page, and authenticate the user by calling the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API action\. After the user is authenticated, Amazon Cognito returns an access token to your app\.

1. Call the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUserAttributeVerificationCode.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUserAttributeVerificationCode.html) API action\. Specify the following parameters in the request:
   + `AccessToken` – The access token returned by Amazon Cognito when the user signed in\.
   + `AttributeName` – Specify `"email"` as the attribute value\.

   Amazon Cognito sends a verification code to the user's email address\.

1. Present a confirmation page where the user enters the verification code\. When the user submits the code, call the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifyUserAttribute.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifyUserAttribute.html) API action\. Specify the following parameters in the request:
   + `AccessToken` – The access token returned by Amazon Cognito when the user signed in\.
   + `AttributeName` – Specify `"email"` as the attribute value\.
   + `Code` – The verification code that the user provided\.

   At this point, the email address is verified\.

## Allowing users to sign up in your app but confirming them as administrator<a name="signing-up-users-in-your-app-and-confirming-them-as-admin"></a>

1. A user signs up in your app by entering a username, phone number and/or email address, and possibly other attributes\.

1. The Amazon Cognito service receives the sign\-up request from the app\. After verifying that the request contains all attributes required for sign\-up, the service completes the sign\-up process and returns to the app that sign\-up is complete, pending confirmation\. At this point the user's account is in an unconfirmed state\. The user cannot sign in until the account is confirmed\.

1. The administrator confirms the user's account, either in the Amazon Cognito console \(by finding the user account in the **Users** tab and choosing the **Confirm** button\) or in the CLI \(by using the `admin-confirm-sign-up` command\)\. Both the **Confirm** button and the `admin-confirm-sign-up` command use the [AdminConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminConfirmSignUp.html) API to perform the confirmation\.

1. At this point the user's account is in a confirmed state, and the user can sign in\.

## Computing SecretHash values<a name="cognito-user-pools-computing-secret-hash"></a>

The following Amazon Cognito User Pools APIs have a `SecretHash` parameter:
+ [ConfirmForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmForgotPassword.html)
+ [ConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html)
+ [ForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html)
+ [ResendConfirmationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ResendConfirmationCode.html)
+ [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)

The `SecretHash` value is a Base 64\-encoded keyed\-hash message authentication code \(HMAC\) calculated using the secret key of a user pool client and username plus the client ID in the message\. The following pseudocode shows how this value is calculated\. In this pseudocode, `+` indicates concatenation, `HMAC_SHA256` represents a function that produces an HMAC value using HmacSHA256, and `Base64` represents a function that produces Base\-64\-encoded version of the hash output\.

```
Base64 ( HMAC_SHA256 ( "Client Secret Key", "Username" + "Client Id" ) )
```

For a detailed overview of how to calculate and use the `SecretHash` parameter, see [How do I troubleshoot "Unable to verify secret hash for client <client\-id>" errors from my Amazon Cognito user pools API?](https://aws.amazon.com/premiumsupport/knowledge-center/cognito-unable-to-verify-secret-hash/) in the AWS Knowledge Center\.

You can use the following code example in your server\-side Java application code:

```
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
 
public static String calculateSecretHash(String userPoolClientId, String userPoolClientSecret, String userName) {
    final String HMAC_SHA256_ALGORITHM = "HmacSHA256";
    
    SecretKeySpec signingKey = new SecretKeySpec(
            userPoolClientSecret.getBytes(StandardCharsets.UTF_8),
            HMAC_SHA256_ALGORITHM);
    try {
        Mac mac = Mac.getInstance(HMAC_SHA256_ALGORITHM);
        mac.init(signingKey);
        mac.update(userName.getBytes(StandardCharsets.UTF_8));
        byte[] rawHmac = mac.doFinal(userPoolClientId.getBytes(StandardCharsets.UTF_8));
        return Base64.getEncoder().encodeToString(rawHmac);
    } catch (Exception e) {
        throw new RuntimeException("Error while calculating ");
    }
}
```

## Confirming user accounts without verifying email or phone number<a name="confirming-user-without-verification-of-email-or-phone-number"></a>

The pre sign\-up Lambda trigger can be used to auto\-confirm user accounts at sign\-up, without requiring a confirmation code or verifying email or phone number\. Users who are confirmed this way can immediately sign in without having to receive a code\.

You can also mark a user's email or phone number verified through this trigger\. 

**Note**  
While this approach is convenient for users when they're getting started, we recommend auto\-verifying at least one of email or phone number\. Otherwise the user can be left unable to recover if they forget their password\.

If you don't require the user to receive and enter a confirmation code at sign\-up and you don't auto\-verify email and phone number in the pre sign\-up Lambda trigger, you risk not having a verified email address or phone number for that user account\. The user can verify the email address or phone number at a later time\. However, if the user forgets his or her password and doesn't have a verified email address or phone number, the user is locked out of the account, because the forgot\-password flow requires a verified email or phone number in order to send a verification code to the user\.

## Verifying when users change their email or phone number<a name="verifying-when-users-change-their-email-or-phone-number"></a>

When a user updates their email address or phone number in your app, Amazon Cognito immediately sends a message with a verification code to a user if you configured your user pool to automatically verify that attribute\. The user must then provide the code from the verification message to your app\. Your app then submits the code in a [VerifyUserAttribute](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifyUserAttribute.html) API request to complete verification of the new attribute value\.

If your user pool doesn’t require that users verify an updated email address or phone number, Amazon Cognito immediately changes the value of an updated `email ` or `phone_number` attribute and marks the attribute as unverified\. Your user can’t sign in with an unverified email or phone number\. They must complete verification of the updated value before they can use that attribute as a sign\-in alias\.

If your user pool requires that users verify an updated email address or phone number, Amazon Cognito leaves the attribute verified and set to its original value until your user verifies the new attribute value\. If the attribute is an alias for sign\-in, your user can sign in with the original attribute value until verification changes the attribute to the new value\. For more information about how to configure your user pool to require users to verify updated attributes, see [Configuring email or phone verification](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-email-phone-verification.html)\.

 You can use a custom message Lambda trigger to customize the verification message\. For more information, see [Custom message Lambda trigger](user-pool-lambda-custom-message.md)\. When a user's email address or phone number is unverified, your app should inform the user that they must verify the attribute, and provide a button or link for users to verify their new email address or phone number\.



## Confirmation and verification processes for user accounts created by administrators or developers<a name="confirmation-and-verification-of-users-whose-accounts-youve-created"></a>

User accounts that are created by an administrator or developer are already in the confirmed state, so users aren't required to enter a confirmation code\. The invitation message that the Amazon Cognito service sends to these users includes the username and a temporary password\. The user is required to change the password before signing in\. For more information, see the [Customize email and SMS messages](how-to-create-user-accounts.md#creating-a-new-user-customize-messages) in [Creating user accounts as administrator](how-to-create-user-accounts.md) and the Custom Message trigger in [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.

## Confirmation and verification processes for imported user accounts<a name="confirmation-and-verification-of-users-whose-accounts-youve-imported"></a>

User accounts that are created by using the user import feature in the AWS Management Console, CLI, or API \(see [Importing users into user pools from a CSV file](cognito-user-pools-using-import-tool.md)\) are already in the confirmed state, so users aren't required to enter a confirmation code\. No invitation message is sent\. However, imported user accounts require users to first request a code by calling the `ForgotPassword` API and then create a password using the delivered code by calling `ConfirmForgotPassword` API before they sign in\. For more information, see [Requiring imported users to reset their passwords](cognito-user-pools-using-import-tool-password-reset.md)\.

Either the user's email or phone number must be marked as verified when the user account is imported, so no verification is required when the user signs in\.

## Sending emails while testing your app<a name="managing-users-accounts-email-testing"></a>

Amazon Cognito sends email messages to your users when they create and manage their accounts in the client app for your user pool\. If you configure your user pool to require email verification, Amazon Cognito sends an email when:
+ A user signs up\.
+ A user updates their email address\.
+ A user performs an action that calls the `ForgotPassword` API action\.
+ You create a user account as an administrator\.

Depending on the action that initiates the email, the email contains a verification code or a temporary password\. Your users must receive these emails and understand the message\. Otherwise, they might be unable to sign in and use your app\.

To ensure that emails send successfully and that the message looks correct, test the actions in your app that initiate email deliveries from Amazon Cognito\. For example, by using the sign\-up page in your app, or by using the `SignUp` API action, you can initiate an email by signing up with a test email address\. When you test in this way, remember the following:

**Important**  
When you use an email address to test actions that initiate emails from Amazon Cognito, don't use a fake email address \(one that has no mailbox\)\. Use a real email address that will receive the email from Amazon Cognito without creating a *hard bounce*\.  
A hard bounce occurs when Amazon Cognito fails to deliver the email to the recipient's mailbox, which always happens if the mailbox doesn't exist\.  
Amazon Cognito limits the number of emails that can be sent by AWS accounts that persistently incur hard bounces\.

When you test actions that initiate emails, use one of the following email addresses to prevent hard bounces:
+ An address for an email account that you own and use for testing\. When you use your own email address, you receive the email that Amazon Cognito sends\. With this email, you can use the verification code to test the sign\-up experience in your app\. If you customized the email message for your user pool, you can check that your customizations look correct\.
+ The mailbox simulator address, *success@simulator\.amazonses\.com*\. If you use the simulator address, Amazon Cognito sends the email successfully, but you're not able to view it\. This option is useful when you don't need to use the verification code and you don't need to check the email message\.
+ The mailbox simulator address with the addition of an arbitrary label, such as *success\+user1@simulator\.amazonses\.com* or *success\+user2@simulator\.amazonses\.com*\. Amazon Cognito emails these addresses successfully, but you're not able to view the emails that it sends\. This option is useful when you want to test the sign\-up process by adding multiple test users to your user pool, and each test user has a unique email address\.