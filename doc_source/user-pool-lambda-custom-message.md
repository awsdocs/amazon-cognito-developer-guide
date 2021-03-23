# Custom Message Lambda Trigger<a name="user-pool-lambda-custom-message"></a>

Amazon Cognito invokes this trigger before sending an email or phone verification message or a multi\-factor authentication \(MFA\) code, allowing you to customize the message dynamically\. Static custom messages can be edited in the **Message Customizations** tab of the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

The request includes `codeParameter`, which is a string that acts as a placeholder for the code that's being delivered to the user\. Insert the `codeParameter` string into the message body, at the position where you want the verification code to be inserted\. On receiving this response, the Amazon Cognito service replaces the `codeParameter` string with the actual verification code\. 

**Note**  
A custom message Lambda function with the `CustomMessage_AdminCreateUser` trigger returns a user name and verification code and so the request must include both `request.usernameParameter` and `request.codeParameter`\. 

**Topics**
+ [Custom Message Lambda Trigger Sources](#cognito-user-pools-lambda-trigger-syntax-custom-message-trigger-source)
+ [Custom Message Lambda Trigger Parameters](#cognito-user-pools-lambda-trigger-syntax-custom-message)
+ [Custom Message for Sign Up Example](#aws-lambda-triggers-custom-message-example)
+ [Custom Message for Admin Create User Example](#aws-lambda-triggers-custom-message-admin-example)

## Custom Message Lambda Trigger Sources<a name="cognito-user-pools-lambda-trigger-syntax-custom-message-trigger-source"></a>


| triggerSource value | Triggering event | 
| --- | --- | 
| CustomMessage\_SignUp | Custom message – To send the confirmation code post sign\-up\. | 
| CustomMessage\_AdminCreateUser | Custom message – To send the temporary password to a new user\. | 
| CustomMessage\_ResendCode | Custom message – To resend the confirmation code to an existing user\. | 
| CustomMessage\_ForgotPassword | Custom message – To send the confirmation code for Forgot Password request\. | 
| CustomMessage\_UpdateUserAttribute | Custom message – When a user's email or phone number is changed, this trigger sends a verification code automatically to the user\. Cannot be used for other attributes\. | 
| CustomMessage\_VerifyUserAttribute | Custom message – This trigger sends a verification code to the user when they manually request it for a new email or phone number\. | 
| CustomMessage\_Authentication | Custom message – To send MFA code during authentication\. | 

## Custom Message Lambda Trigger Parameters<a name="cognito-user-pools-lambda-trigger-syntax-custom-message"></a>

These are the parameters required by this Lambda function in addition to the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-sample-event-parameter-shared)\.

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

### Custom Message Request Parameters<a name="cognito-user-pools-lambda-trigger-syntax-custom-message-request"></a>

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**codeParameter**  
A string for you to use as the placeholder for the verification code in the custom message\.

**usernameParameter**  
The username parameter\. It is a required request parameter for the admin create user flow\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the custom message trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the following API actions:  
+  [AdminResetUserPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminResetUserPassword.html) 
+  [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) 
+  [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html)
+  [ForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html)
+  [GetUserAttributeVerificationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUserAttributeVerificationCode.html)
+  [ResendConfirmationCode](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ResendConfirmationCode.html)
+  [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)
+  [UpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html)

### Custom Message Response Parameters<a name="cognito-user-pools-lambda-trigger-syntax-custom-message-response"></a>

In the response, you specify the custom text to use in messages to your users\.

**smsMessage**  
The custom SMS message to be sent to your users\. Must include the `codeParameter` value received in the request\.

**emailMessage**  
The custom email message to be sent to your users\. Must include the `codeParameter` value received in the request\. If EmailSendingAccount is not DEVELOPER and EmailMessage is returned, 400 error code `com.amazonaws.cognito.identity.idp.model.InvalidLambdaResponseException` will be thrown\. emailMessage is allowed only if UserPool's EmailSendingAccount is DEVELOPER\.

**emailSubject**  
The subject line for the custom message\. If EmailSendingAccount is not DEVELOPER and EmailMessage is returned, 400 error code `com.amazonaws.cognito.identity.idp.model.InvalidLambdaResponseException` will be thrown\. emailSubject is allowed only if UserPool's EmailSendingAccount is DEVELOPER\.

## Custom Message for Sign Up Example<a name="aws-lambda-triggers-custom-message-example"></a>

This Lambda function is invoked to customize an email or SMS message when the service requires an app to send a verification code to the user\.

 A Lambda trigger can be invoked at multiple points: post\-registration, resending a verification code, forgotten password, or verifying a user attribute\. The response includes messages for both SMS and email\. The message must include the code parameter, `"####"`, which is the placeholder for the verification code that is delivered to the user\.

For email, the maximum length for the message is 20,000 UTF\-8 characters, including the verification code\. HTML tags can be used in these emails\.

For SMS, the maximum length is 140 UTF\-8 characters, including the verification code\.

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

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object back to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

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

## Custom Message for Admin Create User Example<a name="aws-lambda-triggers-custom-message-admin-example"></a>

A custom message Lambda function with the `CustomMessage_AdminCreateUser` trigger returns a user name and verification code and so include both `request.usernameParameter` and `request.codeParameter` in the message body\.

The code parameter value `"####"` is a placeholder for the temporary password and "username" is a placeholder for the username delivered to your user\.

For email, the maximum length for the message is 20,000 UTF\-8 characters, including the verification code\. HTML tags can be used in these emails\. For SMS, the maximum length is 140 UTF\-8 characters, including the verification code\.

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

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object back to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

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