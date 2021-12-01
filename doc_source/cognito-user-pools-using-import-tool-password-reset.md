# Requiring imported users to reset their passwords<a name="cognito-user-pools-using-import-tool-password-reset"></a>

The first time each imported user signs in, he or she is required to enter a new password as follows:

**Requiring imported users to reset their passwords**

1. The user attempts to sign in, providing user name and password \(via `InitiateAuth`\)\.

1. Amazon Cognito returns `NotAuthorizedException` when `PreventUserExistenceErrors` is enabled\. Otherwise, it returns `PasswordResetRequiredException`\.

1. The app should direct the user into the `ForgotPassword` flow as outlined in the following procedure:

   1. The app calls `ForgotPassword(user name)`\.

   1. Amazon Cognito sends a code to the verified email or phone number \(depending on what you have provided in the \.csv file for that user\) and indicates to the app where the code was sent in the response to the `ForgotPassword` request\.
**Note**  
For sending reset password codes, it is important that your user pool has phone number or email verification turned on\.

   1. The app indicates to the user that a code was sent and where the code was sent, and the app provides a UI to enter the code and a new password\.

   1. The user enters the code and new password in the app\.

   1. The app calls `ConfirmForgotPassword(code, password)`, which, if successful, sets the new password\.

   1. The app should now direct the user to a sign\-in page\.