# Custom message Lambda trigger<a name="user-pool-lambda-custom-message"></a>

Amazon Cognito invokes this trigger before it sends an email or phone verification message or a multi\-factor authentication \(MFA\) code\. You can customize the message dynamically with your custom message trigger\. You can edit static custom messages in the **Message customizations** tab of the original [Amazon Cognito](https://console.aws.amazon.com/cognito/home) console\.

The request includes `codeParameter`\. This is a string that acts as a placeholder for the code that Amazon Cognito delivers to the user\. Insert the `codeParameter` string into the message body where you want the verification code to appear\. When Amazon Cognito receives this response, Amazon Cognito replaces the `codeParameter` string with the actual verification code\. 

**Note**  
A custom message Lambda function with the `CustomMessage_AdminCreateUser` trigger returns a user name and verification code\. The response must include both `request.usernameParameter` and `request.codeParameter`\. For messages where this is not the case, the default response will be used instead. 

**Topics**
+ [Custom message Lambda trigger sources](#cognito-user-pools-lambda-trigger-syntax-custom-message-trigger-source)
+ [Custom message Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-custom-message)
+ [Custom message for sign\-up example](#aws-lambda-triggers-custom-message-example)
+ [Custom message for admin create user example](#aws-lambda-triggers-custom-message-admin-example)

## Custom message Lambda trigger sources<a name="cognito-user-pools-lambda-trigger-syntax-custom-message-trigger-source"></a>


| triggerSource value | Triggering event | 
| --- | --- | 
| CustomMessage\_SignUp | Custom message – To send the confirmation code post sign\-up\. | 
| CustomMessage\_AdminCreateUser | Custom message – To send the temporary password to a new user\. | 
| CustomMessage\_ResendCode | Custom message – To resend the confirmation code to an existing user\. | 
| CustomMessage\_ForgotPassword | Custom message – To send the confirmation code for Forgot Password request\. | 
| CustomMessage\_UpdateUserAttribute | Custom message – When a user's email or phone number is changed, this trigger sends a verification code automatically to the user\. Cannot be used for other attributes\. | 
| CustomMessage\_VerifyUserAttribute | Custom message – This trigger sends a verification code to the user when they manually request it for a new email or phone number\. | 
| CustomMessage\_Authentication | Custom message – To send MFA code during authentication\. | 

## Custom message Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-custom-message"></a>

These are the parameters that Amazon Cognito passes to this Lambda function along with the event information in the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-syntax-shared)\.

------
#### [ JSON ]

```
{
    "request": {
        "userAttributes": {
            "string": "string",
            . . .
        }
        "codeParameter": "####",
        "usernameParameter": "string",
        "clientMetadata": {
            "string": "string",
            . . .
        }
    },
    "response": {
        "smsMessage": "string",
        "emailMessage": "string",
        "emailSubject": "string"
    }
}
```

------

### Custom message request parameters<a name="cognito-user-pools-lambda-trigger-syntax-custom-message-request"></a>

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**codeParameter**  
A string for you to use as the placeholder for the verification code in the custom message\.

**usernameParameter**  
The username parameter\. This parameter is required as part of the response for the admin create user flow\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the custom message trigger\. The request that invokes a custom message function doesn't include data passed in the ClientMetadata parameter in [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) and [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API operations\. To pass this data to your Lambda function, you can use the ClientMetadata parameter in the following API actions:  
+  [AdminResetUserPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminResetUserPassword.html) 
+  [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) 
+  [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html)
+  [ForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html)
+  [GetUserAttributeVerificationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUserAttributeVerificationCode.html)
+  [ResendConfirmationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ResendConfirmationCode.html)
+  [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)
+  [UpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html)

### Custom message response parameters<a name="cognito-user-pools-lambda-trigger-syntax-custom-message-response"></a>

In the response, specify the custom text to use in messages to your users\.

**smsMessage**  
The custom SMS message to be sent to your users\. Must include the `codeParameter` value that you received in the request\.

**emailMessage**  
The custom email message to send to your users\. You can use HTML formatting in the `emailMessage` parameter\. Must include the `codeParameter` value that you received in the request as the variable `{####}`\. Amazon Cognito can use the `emailMessage` parameter only if the `EmailSendingAccount` attribute of the user pool is `DEVELOPER`\. If the `EmailSendingAccount` attribute of the user pool isn't `DEVELOPER` and an `emailMessage` parameter is returned, Amazon Cognito generates a 400 error code `com.amazonaws.cognito.identity.idp.model.InvalidLambdaResponseException`\. When you choose Amazon Simple Email Service \(Amazon SES\) to send email messages, the `EmailSendingAccount` attribute of a user pool is `DEVELOPER`\. Otherwise, the value is `COGNITO_DEFAULT`\.

**emailSubject**  
The subject line for the custom message\. You can only use the `emailSubject` parameter if the EmailSendingAccount attribute of the user pool is `DEVELOPER`\. If the `EmailSendingAccount` attribute of the user pool isn't `DEVELOPER` and Amazon Cognito returns an `emailSubject` parameter, Amazon Cognito generates a 400 error code `com.amazonaws.cognito.identity.idp.model.InvalidLambdaResponseException`\. The `EmailSendingAccount` attribute of a user pool is `DEVELOPER` when you choose to use Amazon Simple Email Service \(Amazon SES\) to send email messages\. Otherwise, the value is `COGNITO_DEFAULT`\.

## Custom message for sign\-up example<a name="aws-lambda-triggers-custom-message-example"></a>

This example Lambda function customizes an email or SMS message when the service requires an app to send a verification code to the user\.

Amazon Cognito can invoke a Lambda trigger at multiple events: post\-registration, resending a verification code, recovering a forgotten password, or verifying a user attribute\. The response includes messages for both SMS and email\. The message must include the code parameter `"####"`\. This parameter is the placeholder for the verification code that the user receives\.

The maximum length for an email message is 20,000 UTF\-8 characters,\. This length includes the verification code\. You can use HTML tags in these email messages\.

The maximum length of SMS messages is 140 UTF\-8 characters\. This length includes the verification code\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    //
    if(event.userPoolId === "theSpecialUserPool") {
        // Identify why was this function invoked
        if(event.triggerSource === "CustomMessage_SignUp") {
            // Ensure that your message contains event.request.codeParameter. This is the placeholder for code that will be sent
            event.response.smsMessage = "Welcome to the service. Your confirmation code is " + event.request.codeParameter;
            event.response.emailSubject = "Welcome to the service";
            event.response.emailMessage = "Thank you for signing up. " + event.request.codeParameter + " is your verification code";
        }
        // Create custom message for other events
    }
    // Customize messages for other user pools

    // Return to Amazon Cognito
    callback(null, event);
};
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that is relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
  "version": 1,
  "triggerSource": "CustomMessage_SignUp/CustomMessage_ResendCode/CustomMessage_ForgotPassword/CustomMessage_VerifyUserAttribute",
  "region": "<region>",
  "userPoolId": "<userPoolId>",
  "userName": "<userName>",
  "callerContext": {
      "awsSdk": "<calling aws sdk with version>",
      "clientId": "<apps client id>",
      ...
  },
  "request": {
      "userAttributes": {
          "phone_number_verified": false,
          "email_verified": true,
           ...
      },
      "codeParameter": "####"
  },
  "response": {
      "smsMessage": "<custom message to be sent in the message with code parameter>"
      "emailMessage": "<custom message to be sent in the message with code parameter>"
      "emailSubject": "<custom email subject>"
  }
}
```

------

## Custom message for admin create user example<a name="aws-lambda-triggers-custom-message-admin-example"></a>

A custom message Lambda function with the `CustomMessage_AdminCreateUser` trigger returns a user name and verification code\. For this reason, include both `request.usernameParameter` and `request.codeParameter` in the message body\.

The code parameter value `####` is a placeholder for the temporary password, and "username" is a placeholder for the username that your user receives\.

The maximum length of an email message is 20,000 UTF\-8 characters\. This length includes the verification code\. You can use HTML tags in these emails\. The maximum length of SMS messages is 140 UTF\-8 characters\. This length includes the verification code\.

The response includes messages for both SMS and email\. 

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    //
    if(event.userPoolId === "theSpecialUserPool") {
        // Identify why was this function invoked
        if(event.triggerSource === "CustomMessage_AdminCreateUser") {
            // Ensure that your message contains event.request.codeParameter event.request.usernameParameter. This is the placeholder for the code and username that will be sent to your user.
            event.response.smsMessage = "Welcome to the service. Your user name is " + event.request.usernameParameter + " Your temporary password is " + event.request.codeParameter;
            event.response.emailSubject = "Welcome to the service";
            event.response.emailMessage = "Welcome to the service. Your user name is " + event.request.usernameParameter + " Your temporary password is " + event.request.codeParameter;
        }
        // Create custom message for other events
    }
    // Customize messages for other user pools

    // Return to Amazon Cognito
    callback(null, event);
};
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that is relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
  "version": 1,
  "triggerSource": "CustomMessage_AdminCreateUser",
  "region": "<region>",
  "userPoolId": "<userPoolId>",
  "userName": "<userName>",
  "callerContext": {
      "awsSdk": "<calling aws sdk with version>",
      "clientId": "<apps client id>",
      ...
  },
  "request": {
      "userAttributes": {
          "phone_number_verified": false,
          "email_verified": true,
           ...
      },
      "codeParameter": "####",
      "usernameParameter": "username"
  },
  "response": {
      "smsMessage": "<custom message to be sent in the message with code parameter and username parameter>"
      "emailMessage": "<custom message to be sent in the message with code parameter and username parameter>"
      "emailSubject": "<custom email subject>"
  }
}
```

------
