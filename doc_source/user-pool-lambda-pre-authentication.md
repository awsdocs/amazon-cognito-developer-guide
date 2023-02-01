# Pre authentication Lambda trigger<a name="user-pool-lambda-pre-authentication"></a>

Amazon Cognito invokes this trigger when a user attempts to sign in so that you can create custom validation that accepts or denies the authentication request\.

**Note**  
Triggers depend on the user existing in the user pool before Amazon Cognito activates the trigger\.

**Topics**
+ [Pre authentication Lambda flows](#user-pool-lambda-pre-authentication-flows)
+ [Pre authentication Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-pre-auth)
+ [Authentication tutorials](#aws-lambda-triggers-pre-authentication-tutorials)
+ [Pre authentication example](#aws-lambda-triggers-pre-authentication-example)

## Pre authentication Lambda flows<a name="user-pool-lambda-pre-authentication-flows"></a>

### Client authentication flow<a name="user-pool-lambda-pre-authentication-1"></a>

![\[Pre authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Server authentication flow<a name="user-pool-lambda-pre-authentication-2"></a>

![\[Pre authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Pre authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

The request includes client validation data from the `ClientMetadata` values that your app passes to the user pool InitiateAuth and AdminInitiateAuth API operations\.

For more information, see [User pool authentication flow](amazon-cognito-user-pools-authentication-flow.md)\.

## Pre authentication Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-auth"></a>

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
        "userNotFound": boolean
    },
    "response": {}
}
```

------

### Pre authentication request parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-auth-request"></a>

**userAttributes**  
One or more name\-value pairs that represent user attributes\.

**userNotFound**  
When you set `PreventUserExistenceErrors` to `ENABLED` for your user pool client, Amazon Cognito populates this Boolean\.

**validationData**  
One or more key\-value pairs that contain the validation data in the user's sign\-in request\. To pass this data to your Lambda function, use the ClientMetadata parameter in the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API actions\.

### Pre authentication response parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-auth-response"></a>

Amazon Cognito does not expect any additional return information in the response\. Your function can return an error to reject the sign\-in attempt, or use API operations to query and modify your resources\.

## Authentication tutorials<a name="aws-lambda-triggers-pre-authentication-tutorials"></a>

Amazon Cognito activates the pre\-authentication Lambda function before Amazon Cognito signs in a new user\. See these sign\-in tutorials for JavaScript, Android, and iOS\.


| Platform | Tutorial | 
| --- | --- | 
| JavaScript Identity SDK | [Sign in users with JavaScript](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html#tutorial-integrating-user-pools-user-sign-in-javascript) | 
| Android Identity SDK | [Sign in users with Android](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-android.html#tutorial-integrating-user-pools-user-sign-in-android) | 
| iOS Identity SDK | [Sign in users with iOS](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-ios.html#tutorial-integrating-user-pools-authenticate-users-ios) | 

## Pre authentication example<a name="aws-lambda-triggers-pre-authentication-example"></a>

This example function prevents users from a specific user pool app client from signing in to the user pool\. 

------
#### [ Node\.js ]

```
const handler = async (event) => {
  if (
    event.callerContext.clientId === "user-pool-app-client-id-to-be-blocked"
  ) {
    throw new Error("Cannot authenticate users from this user pool app client");
  }

  return event;
};

export { handler };
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

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that is relevant to your Lambda trigger\. The following is a test event for this code sample: 

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