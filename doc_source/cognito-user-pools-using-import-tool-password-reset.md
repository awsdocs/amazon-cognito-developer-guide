# Requiring imported users to reset their passwords<a name="cognito-user-pools-using-import-tool-password-reset"></a>

The first time each imported user signs in, they are required to enter a new password\. The following procedure describes the user experience in a custom app with native users after you import a CSV file\. If your users sign in with the hosted UI, Amazon Cognito prompts them to set a new password when they first sign in\.

**Requiring imported users to reset their passwords**

1. The user attempts to sign in\. Your app calls `InitiateAuth` to submit their user name and password\.

1. Amazon Cognito returns a `NotAuthorizedException` when `PreventUserExistenceErrors` is enabled\. Otherwise, it returns `PasswordResetRequiredException`\.

1. Your app makes a `ForgotPassword` API request and resets the user's password\.

   1. The app submits the user name in a `ForgotPassword` API request\.

   1. Amazon Cognito sends a code to the verified email or phone number\. The destination depends on the values you provided for `email_verified` and `phone_number_verified` in your CSV file\. The response to the `ForgotPassword` request indicates the destination of the code\.
**Note**  
Your user pool must be configured to verify emails or phone numbers\. For more information, see [Signing up and confirming user accounts](signing-up-users-in-your-app.md)\.

   1. Your app displays a message to your user to check the location where the code was sent, and prompts your user to enter the code and a new password\.

   1. The user enters the code and new password in the app\.

   1. The app submits the code and new password in a `ConfirmForgotPassword` API request\.

   1. Your app redirects your user to sign\-in\.