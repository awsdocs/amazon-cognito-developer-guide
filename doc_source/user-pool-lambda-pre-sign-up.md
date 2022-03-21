# Pre sign\-up Lambda trigger<a name="user-pool-lambda-pre-sign-up"></a>

The pre sign\-up Lambda function is triggered just before Amazon Cognito signs up a new user\. It allows you to perform custom validation to accept or deny the registration request as part of the sign\-up process\.

**Topics**
+ [Pre sign\-up Lambda flows](#user-pool-lambda-pre-sign-up-flows)
+ [Pre sign\-up Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-pre-signup)
+ [Sign\-up tutorials](#aws-lambda-triggers-pre-registration-tutorials)
+ [Pre sign\-up example: Auto\-confirm users from a registered domain](#aws-lambda-triggers-pre-registration-example)
+ [Pre sign\-up example: Auto\-confirm and auto\-verify all users](#aws-lambda-triggers-pre-registration-example-2)

## Pre sign\-up Lambda flows<a name="user-pool-lambda-pre-sign-up-flows"></a>

### Client sign\-up flow<a name="user-pool-lambda-pre-sign-up-1"></a>

![\[Pre sign-up Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre sign-up Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre sign-up Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Server sign\-up flow<a name="user-pool-lambda-pre-sign-up-2"></a>

![\[Pre sign-up Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre sign-up Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre sign-up Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

The request includes validation data from the client which comes from the `ValidationData` values passed to the user pool SignUp and AdminCreateUser API methods\.

## Pre sign\-up Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-signup"></a>

These are the parameters that Amazon Cognito passes to this Lambda function along with the event information in the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-syntax-shared)\.

------
#### [ JSON ]

```
{
    "request": {
        "userAttributes": {
            "string": "string",
            . . .
        },
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
        "autoConfirmUser": "boolean",
        "autoVerifyPhone": "boolean",
        "autoVerifyEmail": "boolean"
    }
}
```

------

### Pre sign\-up request parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-signup-request"></a>

**userAttributes**  
One or more name\-value pairs representing user attributes\. The attribute names are the keys\.

**validationData**  
One or more name\-value pairs containing the validation data in the request to register a user\. The validation data is set and then passed from the client in the request to register a user\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API actions\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the pre sign\-up trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the following API actions: [AdminCreateUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminCreateUser.html), [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html), [ForgotPassword](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html), and [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)\.

### Pre sign\-up response parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-signup-response"></a>

In the response, you can set `autoConfirmUser` to `true` if you want to auto\-confirm the user\. You can set `autoVerifyEmail` to `true` to auto\-verify the user's email\. You can set `autoVerifyPhone` to `true` to auto\-verify the user's phone number\.

**Note**  
Response parameters `autoVerifyPhone`, `autoVerifyEmail` and `autoConfirmUser` are ignored by Amazon Cognito when the Pre Sign\-up lambda is triggered by the `AdminCreateUser` API\.

**autoConfirmUser**  
Set to `true` to auto\-confirm the user, or `false` otherwise\.

**autoVerifyEmail**  
Set to `true` to set as verified the email of a user who is signing up, or `false` otherwise\. If `autoVerifyEmail` is set to `true`, the `email` attribute must have a valid, non\-null value\. Otherwise an error will occur and the user will not be able to complete sign\-up\.  
If the `email` attribute is selected as an alias, an alias will be created for the user's email when `autoVerifyEmail` is set\. If an alias with that email already exists, the alias will be moved to the new user and the previous user's email will be marked as unverified\. For more information, see [Aliases](user-pool-settings-attributes.md#user-pool-settings-aliases)\.

**autoVerifyPhone**  
Set to `true` to set as verified the phone number of a user who is signing up, or `false` otherwise\. If `autoVerifyPhone` is set to `true`, the `phone_number` attribute must have a valid, non\-null value\. Otherwise an error will occur and the user will not be able to complete sign\-up\.  
If the `phone_number` attribute is selected as an alias, an alias will be created for the user's phone number when `autoVerifyPhone` is set\. If an alias with that phone number already exists, the alias will be moved to the new user and the previous user's phone number will be marked as unverified\. For more information, see [Aliases](user-pool-settings-attributes.md#user-pool-settings-aliases)\.

## Sign\-up tutorials<a name="aws-lambda-triggers-pre-registration-tutorials"></a>

The pre sign\-up Lambda function is triggered before user sign\-up\. See these Amazon Cognito sign\-up tutorials for JavaScript, Android, and iOS\.


| Platform | Tutorial | 
| --- | --- | 
| JavaScript Identity SDK | [Sign up users with JavaScript](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html#tutorial-integrating-user-pools-sign-up-users-javascript) | 
| Android Identity SDK | [Sign up users with Android](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-android.html#tutorial-integrating-user-pools-sign-up-users-android) | 
| iOS Identity SDK | [Sign up users with iOS](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-ios.html#tutorial-integrating-user-pools-sign-up-users-ios) | 

## Pre sign\-up example: Auto\-confirm users from a registered domain<a name="aws-lambda-triggers-pre-registration-example"></a>

With the pre sign\-up Lambda trigger, you can add custom logic to validate new users signing up for your user pool\. This is a sample JavaScript program that demonstrates how to sign up a new user\. It will invoke a pre sign\-up Lambda trigger as part of the authentication\.

------
#### [ JavaScript ]

```
var attributeList = [];
var dataEmail = {
    Name : 'email',
    Value : '...' // your email here
};
var dataPhoneNumber = {
    Name : 'phone_number',
    Value : '...' // your phone number here with +country code and no delimiters in front
};

var dataEmailDomain = {
    Name: "custom:domain",
    Value: "example.com"
}
var attributeEmail = 
new AmazonCognitoIdentity.CognitoUserAttribute(dataEmail);
var attributePhoneNumber = 
new AmazonCognitoIdentity.CognitoUserAttribute(dataPhoneNumber);
var attributeEmailDomain = 
new AmazonCognitoIdentity.CognitoUserAttribute(dataEmailDomain);
 
attributeList.push(attributeEmail);
attributeList.push(attributePhoneNumber);
attributeList.push(attributeEmailDomain);
 
var cognitoUser;
userPool.signUp('username', 'password', attributeList, null, function(err, result){
    if (err) {
        alert(err);
        return;
    }
    cognitoUser = result.user;
    console.log('user name is ' + cognitoUser.getUsername());
});
```

------

This is a sample Lambda trigger called just before sign\-up with the user pool pre sign\-up Lambda trigger\. It uses a custom attribute **custom:domain** to automatically confirm new users from a particular email domain\. Any new users not in the custom domain will be added to the user pool, but not automatically confirmed\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    // Set the user pool autoConfirmUser flag after validating the email domain
    event.response.autoConfirmUser = false;

    // Split the email address so we can compare domains
    var address = event.request.userAttributes.email.split("@")
    
    // This example uses a custom attribute "custom:domain"
    if (event.request.userAttributes.hasOwnProperty("custom:domain")) {
        if ( event.request.userAttributes['custom:domain'] === address[1]) {
            event.response.autoConfirmUser = true;
        }
    }

    // Return to Amazon Cognito
    callback(null, event);
};
```

------
#### [ Python ]

```
def lambda_handler(event, context):
    # It sets the user pool autoConfirmUser flag after validating the email domain
    event['response']['autoConfirmUser'] = False

    # Split the email address so we can compare domains
    address = event['request']['userAttributes']['email'].split('@')

    # This example uses a custom attribute 'custom:domain'
    if 'custom:domain' in event['request']['userAttributes']:
        if event['request']['userAttributes']['custom:domain'] == address[1]:
            event['response']['autoConfirmUser'] = True

    # Return to Amazon Cognito
    return event
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
    "request": {
        "userAttributes": {
            "email": "testuser@example.com",
            "custom:domain": "example.com"
        }
    },
    "response": {}
}
```

------

## Pre sign\-up example: Auto\-confirm and auto\-verify all users<a name="aws-lambda-triggers-pre-registration-example-2"></a>

This example confirms all users and sets the user's `email` and `phone_number` attributes to verified if the attribute is present\. Also, if aliasing is enabled, aliases will be created for `phone_number` and `email` when auto\-verify is set\. 

**Note**  
If an alias with the same phone number already exists, the alias will be moved to the new user, and the previous user's `phone_number` will be marked as unverified\. The same is true for email addresses\. To prevent this from happening, you can use the user pools [ListUsers API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html) to see if there is an existing user who is already using the new user's phone number or email address as an alias\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {

    // Confirm the user
        event.response.autoConfirmUser = true;

    // Set the email as verified if it is in the request
    if (event.request.userAttributes.hasOwnProperty("email")) {
        event.response.autoVerifyEmail = true;
    }

    // Set the phone number as verified if it is in the request
    if (event.request.userAttributes.hasOwnProperty("phone_number")) {
        event.response.autoVerifyPhone = true;
    }

    // Return to Amazon Cognito
    callback(null, event);
};
```

------
#### [ Python ]

```
def lambda_handler(event, context):
    # Confirm the user
    event['response']['autoConfirmUser'] = True

    # Set the email as verified if it is in the request
    if 'email' in event['request']['userAttributes']:
        event['response']['autoVerifyEmail'] = True

    # Set the phone number as verified if it is in the request
    if 'phone_number' in event['request']['userAttributes']:
        event['response']['autoVerifyPhone'] = True

    # Return to Amazon Cognito
    return event
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
  "request": {
    "userAttributes": {
      "email": "user@example.com",
      "phone_number": "+12065550100"
    }
  },
  "response": {}
}
```

------