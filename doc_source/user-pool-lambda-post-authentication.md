# Post authentication Lambda trigger<a name="user-pool-lambda-post-authentication"></a>

Because Amazon Cognito invokes this trigger after signing in a user, you can add custom logic after Amazon Cognito authenticates the user\.

**Topics**
+ [Post authentication Lambda flows](#user-pool-lambda-post-authentication-flows)
+ [Post authentication Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-post-auth)
+ [Authentication tutorials](#aws-lambda-triggers-post-authentication-tutorials)
+ [Post authentication example](#aws-lambda-triggers-post-authentication-example)

## Post authentication Lambda flows<a name="user-pool-lambda-post-authentication-flows"></a>

### Client authentication flow<a name="user-pool-lambda-post-authentication-1"></a>

![\[Post authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Post authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Post authentication Lambda trigger - client flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

### Server authentication flow<a name="user-pool-lambda-post-authentication-2"></a>

![\[Post authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Post authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Post authentication Lambda trigger - server flow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

For more information, see [User pool authentication flow](amazon-cognito-user-pools-authentication-flow.md)\.

## Post authentication Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-post-auth"></a>

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
         "newDeviceUsed": boolean,
         "clientMetadata": {
             "string": "string",
             . . .
            }
        },
    "response": {}
}
```

------

### Post authentication request parameters<a name="cognito-user-pools-lambda-trigger-syntax-post-auth-request"></a>

**newDeviceUsed**  
This flag indicates if the user has signed in on a new device\. Amazon Cognito only sets this flag if the remembered devices value of the user pool is `Always` or `User Opt-In`\.

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the post authentication trigger\. To pass this data to your Lambda function, you can use the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API actions\. Amazon Cognito doesn't include data from the ClientMetadata parameter in [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) and [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API operations in the request that it passes to the post authentication function\.

### Post authentication response parameters<a name="cognito-user-pools-lambda-trigger-syntax-post-auth-response"></a>

Amazon Cognito doesn't expect any additional return information in the response\. Your function can use API operations to query and modify your resources, or record event metadata to an external system\.

## Authentication tutorials<a name="aws-lambda-triggers-post-authentication-tutorials"></a>

Immediately after Amazon Cognito signs in a user, it activates the post authentication Lambda function\. See these sign\-in tutorials for JavaScript, Android, and iOS\.


| Platform | Tutorial | 
| --- | --- | 
| JavaScript Identity SDK | [Sign in users with JavaScript](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-javascript.html#tutorial-integrating-user-pools-user-sign-in-javascript) | 
| Android Identity SDK | [Sign in users with Android](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-android.html#tutorial-integrating-user-pools-user-sign-in-android) | 
| iOS Identity SDK | [Sign in users with iOS](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-integrating-user-pools-ios.html#tutorial-integrating-user-pools-authenticate-users-ios) | 

## Post authentication example<a name="aws-lambda-triggers-post-authentication-example"></a>

This post authentication sample Lambda function sends data from a successful sign\-in to CloudWatch Logs\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {

    // Send post authentication data to Cloudwatch logs
    console.log ("Authentication successful");
    console.log ("Trigger function =", event.triggerSource);
    console.log ("User pool = ", event.userPoolId);
    console.log ("App client ID = ", event.callerContext.clientId);
    console.log ("User ID = ", event.userName);

    // Return to Amazon Cognito
    callback(null, event);
};
```

------
#### [ Python ]

```
from __future__ import print_function
def lambda_handler(event, context):

    # Send post authentication data to Cloudwatch logs
    print ("Authentication successful")
    print ("Trigger function =", event['triggerSource'])
    print ("User pool = ", event['userPoolId'])
    print ("App client ID = ", event['callerContext']['clientId'])
    print ("User ID = ", event['userName'])

    # Return to Amazon Cognito
    return event
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
  "triggerSource": "testTrigger",
  "userPoolId": "testPool",
  "userName": "testName",
  "callerContext": {
      "clientId": "12345"
  },
  "response": {}
}
```

------