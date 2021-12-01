# Migrate user Lambda trigger<a name="user-pool-lambda-migrate-user"></a>

Amazon Cognito invokes this trigger when a user does not exist in the user pool at the time of sign\-in with a password, or in the forgot\-password flow\. After the Lambda function returns successfully, Amazon Cognito creates the user in the user pool\. For details on the authentication flow with the user migration Lambda trigger see [Importing users into user pools with a user migration Lambda trigger](cognito-user-pools-import-using-lambda.md)\.

You can migrate users from your existing user directory into Amazon Cognito user pools at the time of sign\-in, or during the forgot\-password flow with this Lambda trigger\.

**Topics**
+ [Migrate user Lambda trigger sources](#user-pool-lambda-migrate-user-trigger-source)
+ [Migrate user Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-user-migration)
+ [Example: Migrate a user with an existing password](#aws-lambda-triggers-user-migration-example-1)

## Migrate user Lambda trigger sources<a name="user-pool-lambda-migrate-user-trigger-source"></a>


| triggerSource value | Triggering event | 
| --- | --- | 
| UserMigration\_Authentication | User migration at the time of sign in\. | 
| UserMigration\_ForgotPassword | User migration during forgot\-password flow\. | 

## Migrate user Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-user-migration"></a>

These are the parameters required by this Lambda function in addition to the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-sample-event-parameter-shared)\.

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
The username entered by the user\.

**password**  
The password entered by the user for sign\-in\. It is not set in the forgot\-password flow\.

**validationData**  
One or more key\-value pairs containing the validation data in the user's sign\-in request\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API actions\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the migrate user trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [ForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html) API actions\.

### Migrate user response parameters<a name="cognito-user-pools-lambda-trigger-syntax-user-migration-response"></a>

**userAttributes**  
This field is required\.   
It must contain one or more name\-value pairs representing user attributes to be stored in the user profile in your user pool\. You can include both standard and custom user attributes\. Custom attributes require the `custom:` prefix to distinguish them from standard attributes\. For more information see [Custom attributes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html#user-pool-settings-custom-attributes.html)\.  
In order for users to reset their passwords in the forgot\-password flow, they must have either a verified email or a verified phone number\. Amazon Cognito sends a message containing a reset password code to the email or phone number in the user attributes\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-lambda-migrate-user.html)

**finalUserStatus**  
During sign\-in, this attribute can be set to `CONFIRMED`, or not set, to auto\-confirm your users and allow them to sign\-in with their previous passwords\. This is the simplest experience for the user\.  
If this attribute is set to RESET\_REQUIRED, the user is required to change his or her password immediately after migration at the time of sign\-in, and your client app needs to handle the `PasswordResetRequiredException` during the authentication flow\.  
The password strength policy that is configured for the user pool will not be enforced during migration using Lambda trigger\. If the password doesn't meet the configured password policy, it will still be accepted to allow user migration to continue\. To enforce password strength policy and reject passwords that don't meet the policy, validate the password strength in your code\. Then set finalUserStatus to RESET\_REQUIRED if the password doesn't meet the policy\. 

**messageAction**  
This attribute can be set to "SUPPRESS" to suppress the welcome message usually sent by Amazon Cognito to new users\. If this attribute is not returned, the welcome message will be sent\.

**desiredDeliveryMediums**  
This attribute can be set to "EMAIL" to send the welcome message by email, or "SMS" to send the welcome message by SMS\. If this attribute is not returned, the welcome message will be sent by SMS\.

**forceAliasCreation**  
If this parameter is set to "true" and the phone number or email address specified in the UserAttributes parameter already exists as an alias with a different user, the API call will migrate the alias from the previous user to the newly created user\. The previous user will no longer be able to log in using that alias\.  
If this attribute is set to "false" and the alias exists, the user will not be migrated, and an error is returned to the client app\.  
If this attribute is not returned, it is assumed to be "false"\.

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