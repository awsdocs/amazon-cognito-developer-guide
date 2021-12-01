# Verify Auth challenge response Lambda trigger<a name="user-pool-lambda-verify-auth-challenge-response"></a>

![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Verify auth challenge response**  
Amazon Cognito invokes this trigger to verify if the response from the end user for a custom Auth Challenge is valid or not\. It is part of a user pool [custom authentication flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html#amazon-cognito-user-pools-custom-authentication-flow)\.

The request for this trigger contains the `privateChallengeParameters` and `challengeAnswer` parameters\. The `privateChallengeParameters` values are returned by the Create Auth Challenge Lambda trigger and will contain the expected response from the user\. The `challengeAnswer` parameter contains the user's response for the challenge\.

The response contains the `answerCorrect` attribute, which is set to `true` if the user successfully completed the challenge, or `false` otherwise\.

The challenge loop will repeat until all challenges are answered\.

**Topics**
+ [Verify Auth challenge Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge)
+ [Verify Auth challenge response example](#aws-lambda-triggers-verify-auth-challenge-response-example)

## Verify Auth challenge Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge"></a>

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
        "privateChallengeParameters": {
            "string": "string",
            . . .
        },
        "challengeAnswer": {
            "string": "string",
            . . .
        },
        "clientMetadata": {
            "string": "string",
            . . .
        },
        "userNotFound": boolean
    },
    "response": {
        "answerCorrect": boolean
    }
}
```

------

### Verify Auth challenge request parameters<a name="cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge-request"></a>

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**userNotFound**  
This boolean is populated when `PreventUserExistenceErrors` is set to `ENABLED` for your User Pool client\.

**privateChallengeParameters**  
This parameter comes from the Create Auth Challenge trigger, and is compared against a user’s **challengeAnswer** to determine whether the user passed the challenge\.  
This parameter is only used by the Verify Auth Challenge Response Lambda trigger\. It should contain all of the information that is required to validate the user's response to the challenge\. That includes the `publicChallengeParameters` parameter which contains the question that is presented to the user, and `privateChallengeParameters` which contains the valid answers for the question\.

**challengeAnswer**  
The answer from the user's response to the challenge\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the verify auth challenge trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API actions\.

### Verify Auth challenge response parameters<a name="cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge-response"></a>

**answerCorrect**  
Set to `true` if the user has successfully completed the challenge, or `false` otherwise\. 

## Verify Auth challenge response example<a name="aws-lambda-triggers-verify-auth-challenge-response-example"></a>

In this example, the Lambda function checks whether the user's response to a challenge matches the expected response\. The `answerCorrect` parameter is set to `true` if the user's response matches the expected response\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    if (event.request.privateChallengeParameters.answer == event.request.challengeAnswer) {
        event.response.answerCorrect = true;
    } else {
        event.response.answerCorrect = false;
    }

    // Return to Amazon Cognito
    callback(null, event);
}
```

------