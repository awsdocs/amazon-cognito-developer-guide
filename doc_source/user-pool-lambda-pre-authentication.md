# Pre Authentication Lambda Trigger<a name="user-pool-lambda-pre-authentication"></a>

Amazon Cognito invokes this trigger when a user attempts to sign in, allowing custom validation to accept or deny the authentication request\.

**Note**  
Triggers are dependant on the user existing in the user pool before trigger activation\.

**Topics**
+ [Pre Authentication Lambda Flows](#user-pool-lambda-pre-authentication-flows)
+ [Pre Authentication Lambda Trigger Parameters](#cognito-user-pools-lambda-trigger-syntax-pre-auth)
+ [Authentication Tutorials](#aws-lambda-triggers-pre-authentication-tutorials)
+ [Pre Authentication Example](#aws-lambda-triggers-pre-authentication-example)

## Pre Authentication Lambda Flows<a name="user-pool-lambda-pre-authentication-flows"></a>

### Client Authentication Flow<a name="user-pool-lambda-pre-authentication-1"></a>

![\[Pre authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Server Authentication Flow<a name="user-pool-lambda-pre-authentication-2"></a>

![\[Pre authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

The request includes validation data from the client which comes from the `ClientMetadata` values passed to the user pool InitiateAuth and AdminInitiateAuth API methods\.

For more information, see [User Pool Authentication Flow](amazon-cognito-user-pools-authentication-flow.md)\.

## Pre Authentication Lambda Trigger Parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-auth"></a>

These are the parameters required by this Lambda function in addition to the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-sample-event-parameter-shared)\.

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
        "userNotFound": boolean
    },
    "response": {}
}
```

------

### Pre Authentication Request Parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-auth-request"></a>

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**userNotFound**  
This boolean is populated when `PreventUserExistenceErrors` is set to `ENABLED` for your User Pool client\.

**validationData**  
One or more key\-value pairs containing the validation data in the user's sign\-in request\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API actions\.

### Pre Authentication Response Parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-auth-response"></a>

No additional return information is expected in the response\.

## Authentication Tutorials<a name="aws-lambda-triggers-pre-authentication-tutorials"></a>

The pre authentication Lambda function is triggered just before Amazon Cognito signs in a new user\. See these sign\-in tutorials for JavaScript, Android, and iOS\.


| Platform | Tutorial | 
| --- | --- | 
| JavaScript Identity SDK | [Sign in users with JavaScript](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html#tutorial-integrating-user-pools-user-sign-in-javascript) | 
| Android Identity SDK | [Sign in users with Android](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-android.html#tutorial-integrating-user-pools-user-sign-in-android) | 
| iOS Identity SDK | [Sign in users with iOS](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-ios.html#tutorial-integrating-user-pools-authenticate-users-ios) | 

## Pre Authentication Example<a name="aws-lambda-triggers-pre-authentication-example"></a>

 This sample function prevents users from a specific user pool app client to sign\-in to the user pool\. 

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    if (event.callerContext.clientId === "user-pool-app-client-id-to-be-blocked") {
        var error = new Error("Cannot authenticate users from this user pool app client");

        // Return error to Amazon Cognito
        callback(error, event);
    }

    // Return to Amazon Cognito
    callback(null, event);
};
```

------
#### [ Python ]

```
def lambda_handler(event, context):
    if event['callerContext']['clientId'] == "<user pool app client id to be blocked>":
        raise Exception("Cannot authenticate users from this user pool app client")

    # Return to Amazon Cognito
    return event
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object back to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample: 

------
#### [ JSON ]

```
{
    "callerContext": {
        "clientId": "<user pool app client id to be blocked>"
    },
    "response": {}
}
```

------