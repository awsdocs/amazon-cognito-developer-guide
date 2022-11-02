# Verify Auth challenge response Lambda trigger<a name="user-pool-lambda-verify-auth-challenge-response"></a>

![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Verify auth challenge response**  
Amazon Cognito invokes this trigger to verify if the response from the user for a custom Auth Challenge is valid or not\. It is part of a user pool [custom authentication flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html#amazon-cognito-user-pools-custom-authentication-flow)\.

The request for this trigger contains the `privateChallengeParameters` and `challengeAnswer` parameters\. The Create Auth Challenge Lambda trigger returns `privateChallengeParameters` values, and contains the expected response from the user\. The `challengeAnswer` parameter contains the user's response for the challenge\.

The response contains the `answerCorrect` attribute\. If the user successfully completes the challenge, Amazon Cognito sets the attribute value to `true`\. If the user doesn't successfully complete the challenge, Amazon Cognito sets the value to `false`\.

The challenge loop repeats until the users answers all challenges\.

**Topics**
+ [Verify Auth challenge Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge)
+ [Verify Auth challenge response example](#aws-lambda-triggers-verify-auth-challenge-response-example)

## Verify Auth challenge Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge"></a>

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
        "privateChallengeParameters": {
            "string": "string",
            . . .
        },
        "challengeAnswer": "string",
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
This parameter contains one or more name\-value pairs that represent user attributes\.

**userNotFound**  
When Amazon Cognito sets `PreventUserExistenceErrors` to `ENABLED` for your user pool client, Amazon Cognito populates this Boolean \.

**privateChallengeParameters**  
This parameter comes from the Create Auth Challenge trigger\. To determine whether the user passed a challenge, Amazon Cognito compares the parameters against a user’s **challengeAnswer**\.  
This parameter contains all of the information that is required to validate the user's response to the challenge\. That information includes the question that Amazon Cognito presents to the user \(`publicChallengeParameters`\), and the valid answers for the question \(`privateChallengeParameters`\)\. Only the Verify Auth Challenge Response Lambda trigger uses this parameter\. 

**challengeAnswer**  
This parameter value is the answer from the user's response to the challenge\.

**clientMetadata**  
This parameter contains one or more key\-value pairs that you can provide as custom input to the Lambda function for the verify auth challenge trigger\. To pass this data to your Lambda function, use the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API operations\. Amazon Cognito doesn't include data from the ClientMetadata parameter in [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) and [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API operations in the request that it passes to the verify auth challenge function\.

### Verify Auth challenge response parameters<a name="cognito-user-pools-lambda-trigger-syntax-verify-auth-challenge-response"></a>

**answerCorrect**  
If the user successfully completes the challenge, Amazon Cognito sets this parameter to `true`\. If the user doesn't successfully complete the challenge, Amazon Cognito sets the parameter to `false`\. 

## Verify Auth challenge response example<a name="aws-lambda-triggers-verify-auth-challenge-response-example"></a>

In this example, the Lambda function checks whether the user's response to a challenge matches the expected response\. If the user's response matches the expected response, Amazon Cognito sets the `answerCorrect` parameter to `true`\.

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