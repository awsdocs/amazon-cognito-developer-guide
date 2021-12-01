# Create Auth challenge Lambda trigger<a name="user-pool-lambda-create-auth-challenge"></a>

![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Create auth challenge**  
Amazon Cognito invokes this trigger after **Define Auth Challenge** if a custom challenge has been specified as part of the **Define Auth Challenge** trigger\. It creates a [custom authentication flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html#amazon-cognito-user-pools-custom-authentication-flow)\.

This Lambda trigger is invoked to create a challenge to present to the user\. The request for this Lambda trigger includes the `challengeName` and `session`\. The `challengeName` is a string and is the name of the next challenge to the user\. The value of this attribute is set in the Define Auth Challenge Lambda trigger\.

The challenge loop will repeat until all challenges are answered\.

**Topics**
+ [Create Auth challenge Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-create-auth-challenge)
+ [Create Auth challenge example](#aws-lambda-triggers-create-auth-challenge-example)

## Create Auth challenge Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-create-auth-challenge"></a>

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
        "challengeName": "string",
        "session": [
            ChallengeResult,
            . . .
        ],
        "clientMetadata": {
            "string": "string",
            . . .
        },
        "userNotFound": boolean
    },
    "response": {
        "publicChallengeParameters": {
            "string": "string",
            . . .
        },
        "privateChallengeParameters": {
            "string": "string",
            . . .
        },
        "challengeMetadata": "string"
    }
}
```

------

### Create Auth challenge request parameters<a name="cognito-user-pools-lambda-trigger-syntax-create-auth-challenge-request"></a>

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**userNotFound**  
This boolean is populated when `PreventUserExistenceErrors` is set to `ENABLED` for your User Pool client\.

**challengeName**  
The name of the new challenge\.

**session**  
The session element is an array of `ChallengeResult` elements, each of which contains the following elements:    
**challengeName**  
The challenge type\. One of: `"CUSTOM_CHALLENGE"`, `"PASSWORD_VERIFIER"`, `"SMS_MFA"`, `"DEVICE_SRP_AUTH"`, `"DEVICE_PASSWORD_VERIFIER"`, or `"ADMIN_NO_SRP_AUTH"`\.   
**challengeResult**  
Set to `true` if the user successfully completed the challenge, or `false` otherwise\.  
**challengeMetadata**  
Your name for the custom challenge\. Used only if `challengeName` is `"CUSTOM_CHALLENGE"`\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the create auth challenge trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API actions\.

### Create Auth challenge response parameters<a name="cognito-user-pools-lambda-trigger-syntax-create-auth-challenge-response"></a>

**publicChallengeParameters**  
One or more key\-value pairs for the client app to use in the challenge to be presented to the user\. This parameter should contain all of the necessary information to accurately present the challenge to the user\.

**privateChallengeParameters**  
This parameter is only used by the Verify Auth Challenge Response Lambda trigger\. This parameter should contain all of the information that is required to validate the user's response to the challenge\. In other words, the `publicChallengeParameters` parameter contains the question that is presented to the user and `privateChallengeParameters` contains the valid answers for the question\.

**challengeMetadata**  
Your name for the custom challenge, if this is a custom challenge\.

## Create Auth challenge example<a name="aws-lambda-triggers-create-auth-challenge-example"></a>

A CAPTCHA is created as a challenge to the user\. The URL for the CAPTCHA image is added to the public challenge parameters as "`captchaUrl`", and the expected answer is added to the private challenge parameters\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    if (event.request.challengeName == 'CUSTOM_CHALLENGE') {
        event.response.publicChallengeParameters = {};
        event.response.publicChallengeParameters.captchaUrl = 'url/123.jpg'
        event.response.privateChallengeParameters = {};
        event.response.privateChallengeParameters.answer = '5';
        event.response.challengeMetadata = 'CAPTCHA_CHALLENGE';
    }

    //Return to Amazon Cognito.
    callback(null, event);
}
```

------