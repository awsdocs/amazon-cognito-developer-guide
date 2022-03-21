# Migrate user Lambda trigger<a name="user-pool-lambda-migrate-user"></a>

When a user doesn't exist in the user pool at sign\-in with a password, or in the forgot\-password flow, Amazon Cognito invokes this trigger\. After the Lambda function returns successfully, Amazon Cognito creates the user in the user pool\. For details on the authentication flow with the user migration Lambda trigger, see [Importing users into user pools with a user migration Lambda trigger](cognito-user-pools-import-using-lambda.md)\.

To migrate users from your existing user directory into Amazon Cognito user pools at sign\-in, or during the forgot\-password flow, use this Lambda trigger\.

**Topics**
+ [Migrate user Lambda trigger sources](#user-pool-lambda-migrate-user-trigger-source)
+ [Migrate user Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-user-migration)
+ [Example: Migrate a user with an existing password](#aws-lambda-triggers-user-migration-example-1)

## Migrate user Lambda trigger sources<a name="user-pool-lambda-migrate-user-trigger-source"></a>


| triggerSource value | Triggering event | 
| --- | --- | 
| UserMigration\_Authentication | User migration at sign\-in\. | 
| UserMigration\_ForgotPassword | User migration during forgot\-password flow\. | 

## Migrate user Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-user-migration"></a>

These are the parameters that Amazon Cognito passes to this Lambda function along with the event information in the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-syntax-shared)\.

------
#### [ JSON ]

```
{
    "userName": "string",
    "request": {
        "password": "string",
        "validationData": {
            "string": "string",
            . . .
        },
        "clientMetadata": {
            "string": "string",
      	. . .
        }
    },
    "response": {
        "userAttributes": {
            "string": "string",
            . . .
        },
        "finalUserStatus": "string",
        "messageAction": "string",
        "desiredDeliveryMediums": [ "string", . . .],
        "forceAliasCreation": boolean
    }
}
```

------

### Migrate user request parameters<a name="cognito-user-pools-lambda-trigger-syntax-user-migration-request"></a>

**userName**  
The username that the user enters at sign\-in\.

**password**  
The password that the user enters at sign\-in\. Amazon Cognito doesn't send this value in a request that's initiated by a forgot\-password flow\.

**validationData**  
One or more key\-value pairs that contain the validation data in the user's sign\-in request\. To pass this data to your Lambda function, you can use the ClientMetadata parameter in the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API actions\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function for the migrate user trigger\. To pass this data to your Lambda function, you can use the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [ForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html) API actions\.

### Migrate user response parameters<a name="cognito-user-pools-lambda-trigger-syntax-user-migration-response"></a>

**userAttributes**  
This field is required\.   
This field must contain one or more name\-value pairs that Amazon Cognito stores in the user profile in your user pool and uses as user attributes\. You can include both standard and custom user attributes\. Custom attributes require the `custom:` prefix to distinguish them from standard attributes\. For more information, see [Custom attributes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-custom-attributes.html)\.  
To reset their passwords in the forgot\-password flow, a user must have either a verified email address or a verified phone number\. Amazon Cognito sends a message containing a reset password code to the email address or phone number in the user attributes\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-migrate-user.html)

**finalUserStatus**  
You can set this parameter to `CONFIRMED` to auto\-confirm your users so that they can sign in with their previous passwords\. When you set a user to `CONFIRMED`, they do not need to take additional action before they can sign in\. If you don't set this attribute to `CONFIRMED`, it's set to `RESET_REQUIRED`\.  
A `finalUserStatus` of `RESET_REQUIRED` means that the user must change their password immediately after migration at sign\-in, and your client app must handle the `PasswordResetRequiredException` during the authentication flow\.  
Amazon Cognito doesn't enforce the password strength policy that you configured for the user pool during migration using Lambda trigger\. If the password doesn't meet the password policy that you configured, Amazon Cognito still accepts the password so that it can continue to migrate the user\. To enforce password strength policy and reject passwords that don't meet the policy, validate the password strength in your code\. Then, if the password doesn't meet the policy, set finalUserStatus to `RESET_REQUIRED`\.

**messageAction**  
You can set this parameter to `SUPPRESS` to decline to send the welcome message that Amazon Cognito usually sends to new users\. If your function doesn't return this parameter, Amazon Cognito sends the welcome message\.

**desiredDeliveryMediums**  
You can set this parameter to `EMAIL` to send the welcome message by email, or `SMS` to send the welcome message by SMS\. If your function doesn't return this parameter, Amazon Cognito sends the welcome message by SMS\.

**forceAliasCreation**  
If you set this parameter to `TRUE` and the phone number or email address in the UserAttributes parameter already exists as an alias with a different user, the API call migrates the alias from the previous user to the newly created user\. The previous user can no longer log in using that alias\.  
If you set this parameter to `FALSE` and the alias exists, Amazon Cognito doesn't migrate the user and returns an error to the client app\.  
If you don't return this parameter, Amazon Cognito assumes its value is "false"\.

## Example: Migrate a user with an existing password<a name="aws-lambda-triggers-user-migration-example-1"></a>

This example Lambda function migrates the user with an existing password and suppresses the welcome message from Amazon Cognito\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {

    var user;

    if ( event.triggerSource == "UserMigration_Authentication" ) {

        // authenticate the user with your existing user directory service
        user = authenticateUser(event.userName, event.request.password);
        if ( user ) {
            event.response.userAttributes = {
                "email": user.emailAddress,
                "email_verified": "true"
            };
            event.response.finalUserStatus = "CONFIRMED";
            event.response.messageAction = "SUPPRESS";
            context.succeed(event);
        }
        else {
            // Return error to Amazon Cognito
            callback("Bad password");
        }
    }
    else if ( event.triggerSource == "UserMigration_ForgotPassword" ) {

        // Lookup the user in your existing user directory service
        user = lookupUser(event.userName);
        if ( user ) {
            event.response.userAttributes = {
                "email": user.emailAddress,
                // required to enable password-reset code to be sent to user
                "email_verified": "true"  
            };
            event.response.messageAction = "SUPPRESS";
            context.succeed(event);
        }
        else {
            // Return error to Amazon Cognito
            callback("Bad password");
        }
    }
    else { 
        // Return error to Amazon Cognito
        callback("Bad triggerSource " + event.triggerSource);
    }
};
```

------