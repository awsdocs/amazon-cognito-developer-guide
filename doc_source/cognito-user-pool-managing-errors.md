# Managing error responses<a name="cognito-user-pool-managing-errors"></a>

Amazon Cognito supports customizing error responses returned by user pools\. Custom error responses are available for user creation and authentication, password recovery, and confirmation operations\.

Use the `PreventUserExistenceErrors` setting of a user pool app client to enable or disable user existence related errors\. When you create a new user pool, `PreventUserExistenceErrors` is `false` by default\. Set this value to `true` to prevent user\-enumeration attacks\. In the Amazon Cognito API, update this value with a [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html) request\. In the AWS Management Console, create or edit your app client to activate **Prevent user existence errors**\.

When you enable custom error responses, Amazon Cognito authentication APIs return a generic authentication failure response\. The error response tells you the user name or password is incorrect\. Amazon Cognito account confirmation and password recovery APIs return a response indicating a code was sent to a simulated delivery medium\. The error response works when the status is `ENABLED` and the user doesn't exist\. Below are the detailed behaviors for the Amazon Cognito operations when `PreventUserExistenceErrors` is set to `ENABLED`\.

## User creation and authentication operations<a name="cognito-user-pool-managing-errors-user-auth"></a>

You can use either *User name password authentication* or *Secure Remote Password \(SRP\) authentication* with the following operations\. You can also customize the errors that you return with custom authentication\.
+ `AdminInitiateAuth`
+ `AdminRespondToAuthChallenge`
+ `InitiateAuth`
+ `RespondToAuthChallenge`

The following list demonstrates how you can customize error responses in user authentication operations\.

**User name and password authentication**  
To sign a user in with `ADMIN_USER_PASSWORD_AUTH` and `USER_PASSWORD_AUTH`, include the user name and password in an `AdminInitiateAuth` or `InitiateAuth` API request\. Amazon Cognito returns a generic `NotAuthorizedException` error when either the user name or password is incorrect\.

**Secure Remote Password \(SRP\) based authentication**  
To sign a user in with `USER_SRP_AUTH`, include a user name and an `SRP_A` parameter in an `AdminInitiateAuth` or `InitiateAuth` API request\. In response, Amazon Cognito returns `SRP_B` and salt for the user, in accord with the SRP standard\. When a user isn't found, Amazon Cognito returns a simulated response in the first step as described in [RFC 5054](https://tools.ietf.org/html/rfc5054#section-2.5.1.3)\. Amazon Cognito returns the same salt and an internal user ID in [Universally Unique Identifier \(UUID\)](https://tools.ietf.org/html/rfc4122) format for the same user name and user pool combination\. When you send a `RespondToAuthChallenge` API request with proof of password, Amazon Cognito returns a generic `NotAuthorizedException` error when either user name or password is incorrect\.  
You can simulate a generic response with user name and password authentication if you are using verification\-based alias attributes, and the immutable user name isn't formatted as a UUID\.

**Custom Authentication Challenge Lambda trigger**  
If you use the [Custom Authentication Challenge Lambda Trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-challenge.html) and you enable error responses, then `LambdaChallenge` returns a boolean parameter named `UserNotFound`\. Then it's passed in the request of `DefineAuthChallenge`, `VerifyAuthChallenge`, and `CreateAuthChallenge` Lambda triggers\. You can use this trigger to simulate custom authorization challenges for a user that doesn't exist\. If you call the Pre\-Authentication Lambda trigger for a user that doesn't exist, then Amazon Cognito returns `UserNotFound`\. 

The following list demonstrates how you can customize error responses in user creation operations\.

**SignUp**  
The `SignUp` operation returns `UsernameExistsException` when a user name is already taken\. If you don't want Amazon Cognito to return a `UsernameExistsException` error for email addresses and phone numbers when you sign up users in your app, use verification\-based alias attributes\. For more information about aliases, see the *Customizing sign\-in attributes* section of [User pool attributes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-aliases)\.  
For an example of how Amazon Cognito can prevent the use of `SignUp` API requests to discover users in your user pool, see [Preventing `UsernameExistsException` errors for email addresses and phone numbers on sign\-up](#cognito-user-pool-managing-errors-prevent-userexistence-errors)\.

**Imported users**  
If `PreventUserExistenceErrors` is enabled, during authentication of imported users a generic `NotAuthorizedException` error is returned indicating either the user name or password was incorrect instead of returning `PasswordResetRequiredException`\. See [Requiring imported users to reset their passwords](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-using-import-tool-password-reset.html) for more information\.

**Migrate user Lambda trigger**  
Amazon Cognito returns a simulated response for users that don't exist when an empty response was set in the original event context by the Lambda trigger\. For more information, see [Migrate User Lambda Trigger](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-migrate-user.html)\. 

### Preventing `UsernameExistsException` errors for email addresses and phone numbers on sign\-up<a name="cognito-user-pool-managing-errors-prevent-userexistence-errors"></a>

The following example demonstrates how, when you configure alias attributes in your user pool, you can keep duplicate email addresses and phone numbers from generating `UsernameExistsException` errors in response to `SignUp` API requests\. You must have created your user pool with email address or phone number as an alias attribute\. For more information, see the *Customizing sign\-in attributes* section of [User pool attributes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-aliases)\.

1. Jie signs up for a new user name, and also provides the email address `jie@example.com`\. Amazon Cognito sends a code to their email address\.

   **Example AWS CLI command**

   ```
   aws cognito-idp sign-up --client-id 1234567890abcdef0 --username jie --password PASSWORD --user-attributes Name="email",Value="jie@example.com"
   ```

   **Example response**

   ```
   {
       "UserConfirmed": false, 
       "UserSub": "<subId>", 
       "CodeDeliveryDetails": {
           "AttributeName": "email", 
           "Destination": "j****@e****", 
           "DeliveryMedium": "EMAIL"
       }
   }
   ```

1. Jie provides the code sent to them to confirm their ownership of the email address\. This completes their registration as a user\.

   **Example AWS CLI command**

   ```
   aws cognito-idp confirm-sign-up --client-id 1234567890abcdef0 --username=jie --confirmation-code xxxxxx
   ```

1. Shirley registers a new user account and provides the email address `jie@example.com`\. Amazon Cognito doesn't return a `UsernameExistsException` error, and sends a confirmation code to Jie's email address\.

   **Example AWS CLI command**

   ```
   aws cognito-idp sign-up --client-id 1234567890abcdef0 --username shirley --password PASSWORD --user-attributes Name="email",Value="jie@example.com"
   ```

   **Example response**

   ```
   {
       "UserConfirmed": false, 
       "UserSub": "<new subId>", 
       "CodeDeliveryDetails": {
           "AttributeName": "email", 
           "Destination": "j****@e****", 
           "DeliveryMedium": "EMAIL"
       }
   }
   ```

1. In a different scenario, Shirley has ownership of `jie@example.com`\. Shirley retrieves the code that Amazon Cognito sent to Jie's email address and attempts to confirm the account\.

   **Example AWS CLI command**

   ```
   aws cognito-idp confirm-sign-up --client-id 1234567890abcdef0 --username=shirley --confirmation-code xxxxxx
   ```

   **Example response**

   ```
   An error occurred (AliasExistsException) when calling the ConfirmSignUp operation: An account with the email already exists.
   ```

Amazon Cognito doesn't return an error to Shirley's `aws cognito-idp sign-up` request, despite `jie@example.com` being assigned to an existing user\. Shirley must demonstrate ownership of the email address before Amazon Cognito returns an error response\. In a user pool with alias attributes, this behavior prevents use of the public `SignUp` API to check whether a user exists with a given email address or phone number\.

This behavior is different from the response that Amazon Cognito returns to `SignUp` request with an existing user name, as shown in the following example\. While Shirley learns from this response that a user already exists with the user name `jie`, they don't learn about any email addresses or phone numbers associated with the user\.

**Example CLI command**

```
aws cognito-idp sign-up --client-id 1example23456789 --username jie --password PASSWORD
      --user-attributes Name="email",Value="shirley@example.com"
```

**Example response**

```
An error occurred (UsernameExistsException) when calling the SignUp operation: User already exists
```

## Password reset operations<a name="cognito-user-pool-managing-errors-password-reset"></a>

The following list demonstrates how you can customize error responses in user password reset operations\.

**ForgotPassword**  
When a user isn't found, is deactivated, or doesn't have a verified delivery mechanism to recover their password, Amazon Cognito returns `CodeDeliveryDetails` with a simulated delivery medium for a user\. The simulated delivery medium is determined by the input user name format and verification settings of the user pool\.

**ConfirmForgotPassword**  
Amazon Cognito returns the `CodeMismatchException` error for users that don't exist or are disabled\. If a code isn't requested when using `ForgotPassword`, Amazon Cognito returns the `ExpiredCodeException` error\.

## Confirmation operations<a name="cognito-user-pool-managing-errors-confirmation"></a>

The following list demonstrates how you can customize error responses in user confirmation and verification operations\.

**ResendConfirmationCode**  
Amazon Cognito returns `CodeDeliveryDetails` for a disabled user or a user that doesn't exist\. Amazon Cognito sends a confirmation code to the existing user's email or phone number\.

**ConfirmSignUp**  
 `ExpiredCodeException` returns if a code has expired\. Amazon Cognito returns `NotAuthorizedException` when a user isn't authorized\. If the code doesn't match what the server expects Amazon Cognito returns `CodeMismatchException`\. 