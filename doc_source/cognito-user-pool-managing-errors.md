# Managing error responses<a name="cognito-user-pool-managing-errors"></a>

Amazon Cognito supports customizing error responses returned by User Pools\. Custom error responses are available for authentication, confirmation, and password recovery\-related operations\. Use the `PreventUserExistenceErrors` setting of a user pool app client to enable or disable user existence related errors\. 

When you enable custom error responses, Amazon Cognito authentication APIs return a generic authentication failure response\. The error response tells you the user name or password is incorrect\. Amazon Cognito account confirmation and password recovery APIs return a response indicating a code was sent to a simulated delivery medium\. The error response works when the status is `ENABLED` and the user doesn't exist\. Below are the detailed behaviors for the Amazon Cognito operations when `PreventUserExistenceErrors` is set to `ENABLED`\.

**User authentication operations**

You can use either authentication flow method with the following operations\.
+ `AdminInitiateAuth`
+ `AdminRespondToAuthChallenge`
+ `InitiateAuth`
+ `RespondToAuthChallenge`

**User name password based authentication**  
In the authentication flows for `ADMIN_USER_PASSWORD_AUTH` and `USER_PASSWORD_AUTH` the user name and password returns with a single call of `InitiateAuth`\. Amazon Cognito returns a generic `NotAuthorizedException` error indicating either the user name or password is incorrect\.

**Secure Remote Password \(SRP\) based authentication**  
In the USER\_SRP\_AUTH authentication flow Amazon Cognito receives a user name and SRP parameter ‘A’ in the first step\. In response, Amazon Cognito returns SRP parameter ‘B’ and ‘salt’ for the user as per SRP protocol\. When a user isn't found, Amazon Cognito returns a simulated response in the first step as described in [RFC 5054](https://tools.ietf.org/html/rfc5054#section-2.5.1.3)\. Amazon Cognito returns the same ‘salt’ and internal user id in [Universally Unique IDentifier \(UUID\)](https://tools.ietf.org/html/rfc4122) format for the same user name and user pool combination\. When the next operation of `RespondToAuthChallenge` proof of password runs, Amazon Cognito returns a generic `NotAuthorizedException` error indicating either user name or password was incorrect\.  
You can use `UsernamePassword` to simulate a generic response if you are using verification based aliases and the format of immutable user name isn't a UUID\.

**ForgotPassword**  
When a user isn't found, is deactivated, or doesn't have a verified delivery mechanism to recover their password, Amazon Cognito returns `CodeDeliveryDetails` with a simulated delivery medium for a user\. The simulated delivery medium is determined by the input user name format and verification settings of the user pool\.

**ConfirmForgotPassword**  
Amazon Cognito returns the `CodeMismatchException` error for users that don't exist or are disabled\. If a code isn't requested when using `ForgotPassword`, Amazon Cognito returns the `ExpiredCodeException` error\.

**ResendConfirmationCode**  
Amazon Cognito returns `CodeDeliveryDetails` for a disabled user or a user that doesn't exist\. Amazon Cognito sends a confirmation code to the existing user's email or phone number\.

**ConfirmSignUp**  
 `ExpiredCodeException` returns if a code has expired\. Amazon Cognito returns `NotAuthorizedException` when a user isn't authorized\. If the code doesn't match what the server expects Amazon Cognito returns `CodeMismatchException`\. 

**SignUp**  
The `SignUp` operation returns `UsernameExistsException` when a user name is already taken\. To prevent the `UsernameExistsException` error for email or phone number during `SignUp`, you can use verification based aliases\. For more information, see [AliasAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) Amazon Cognito API Reference guide\. For more information about aliases see [Overview of Aliases](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-aliases)\. 

**Imported users**  
If `PreventUserExistenceErrors` is enabled, during authentication of imported users a generic `NotAuthorizedException` error is returned indicating either the user name or password was incorrect instead of returning `PasswordResetRequiredException`\. See [Requiring imported users to reset their passwords](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-using-import-tool-password-reset.html) for more information\.

**Migrate user Lambda trigger**  
Amazon Cognito returns a simulated response for users that don't exist when an empty response was set in the original event context by the Lambda trigger\. For more information, see [Migrate User Lambda Trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-migrate-user.html)\. 

**Custom Authentication Challenge Lambda trigger**  
If you use [Custom Authentication Challenge Lambda Trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-challenge.html) and you enable error responses, then `LambdaChallenge` returns a boolean parameter named `UserNotFound`\. Then it's passed in the request of `DefineAuthChallenge`, `VerifyAuthChallenge`, and `CreateAuthChallenge` Lambda triggers\. You can use this trigger to simulate custom authorization challenges for a user that doesn't exist\. If you call the Pre\-Authentication Lambda trigger for a user that doesn't exist, then Amazon Cognito returns `UserNotFound`\. 