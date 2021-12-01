# Define Auth challenge Lambda trigger<a name="user-pool-lambda-define-auth-challenge"></a>

![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Define auth challenge**  
 Amazon Cognito invokes this trigger to initiate the [custom authentication flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html#amazon-cognito-user-pools-custom-authentication-flow)\.

The request for this Lambda trigger contains `session`, which is an array that contains all of the challenges that are presented to the user in the current authentication process\. The request also includes the corresponding result\. The challenge details \(`ChallengeResult`\) are stored in chronological order in the `session` array, with `session[0]` representing the first challenge that is presented to the user\.

 You can have Amazon Cognito verify user passwords before it issues your custom challenges\. Here is an overview of the process:

1. To start, have your app initiate sign\-in by calling `InitiateAuth` or `AdminInitiateAuth` with the `AuthParameters` map, including `CHALLENGE_NAME: SRP_A,` and values for `SRP_A` and `USERNAME`\. 

1. Your define auth challenge Lambda trigger is invoked with an initial session that contains `challengeName: SRP_A` and `challengeResult: true`\. 

1. After receiving those inputs, your Lambda function responds with `challengeName: PASSWORD_VERIFIER`, `issueTokens: false`, `failAuthentication: false`\. 

1. If the password verification succeeds, your Lambda function is invoked again with a new session containing `challengeName: PASSWORD_VERIFIER` and `challengeResult: true`\. 

1. Your Lambda function initiates your custom challenges by responding with `challengeName: CUSTOM_CHALLENGE`, `issueTokens: false`, and `failAuthentication: false`\. If you don't want to start your custom auth flow with password verification, you can initiate sign\-in with the `AuthParameters` map including `CHALLENGE_NAME: CUSTOM_CHALLENGE`\. 

1. The challenge loop repeats until all challenges are answered\.

**Topics**
+ [Define Auth challenge Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-define-auth-challenge)
+ [Define Auth challenge example](#aws-lambda-triggers-define-auth-challenge-example)

## Define Auth challenge Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-define-auth-challenge"></a>

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
        "challengeName": "string",
        "issueTokens": boolean,
        "failAuthentication": boolean
}
```

------

### Define Auth challenge request parameters<a name="cognito-user-pools-lambda-trigger-syntax-define-auth-challenge-request"></a>

 These are the parameters provided to your Lambda function when it is invoked\.

**userAttributes**  
One or more name\-value pairs representing user attributes\.

**userNotFound**  
A Boolean that is populated when `PreventUserExistenceErrors` is set to `ENABLED` for your user pool client\. A value of `true` means that the user id \(user name, email address, etc\.\) did not match any existing users\. When `PreventUserExistenceErrors` is set to `ENABLED`, the service will not report back to the app that the user does not exist\. The recommended best practice is for your Lambda functions to maintain the same user experience including latency so the caller cannot detect different behavior when the user exists or doesn’t exist\.

**session**  
an array of `ChallengeResult` elements, each of which contains the following elements:    
**challengeName**  
The challenge type\. One of: `CUSTOM_CHALLENGE`, `SRP_A`, `PASSWORD_VERIFIER`, `SMS_MFA, DEVICE_SRP_AUTH`, `DEVICE_PASSWORD_VERIFIER`, or `ADMIN_NO_SRP_AUTH`\.   
You should always check `challengeName` in your `DefineAuthChallenge` Lambda trigger to make sure it matches the expected value when determining if a user has successfully authenticated and should be issued tokens\.  
**challengeResult**  
Set to `true` if the user successfully completed the challenge, or `false` otherwise\.  
**challengeMetadata**  
Your name for the custom challenge\. Used only if `challengeName` is `CUSTOM_CHALLENGE`\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the define auth challenge trigger\. You can pass this data to your Lambda function by using the `ClientMetadata` parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API operations\.

### Define Auth challenge response parameters<a name="cognito-user-pools-lambda-trigger-syntax-define-auth-challenge-response"></a>

In the response, you can return the next stage of the authentication process\.

**challengeName**  
A string containing the name of the next challenge\. If you want to present a new challenge to your user, specify the challenge name here\.

**issueTokens**  
Set to `true` if you determine that the user has been sufficiently authenticated by completing the challenges, or `false` otherwise\.

**failAuthentication**  
Set to `true` if you want to terminate the current authentication process, or `false` otherwise\.

## Define Auth challenge example<a name="aws-lambda-triggers-define-auth-challenge-example"></a>

This example defines a series of challenges for authentication and issues tokens only if all of the challenges are successfully completed\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    if (event.request.session.length == 1 && event.request.session[0].challengeName == 'SRP_A') {
        event.response.issueTokens = false;
        event.response.failAuthentication = false;
        event.response.challengeName = 'PASSWORD_VERIFIER';
    } else if (event.request.session.length == 2 && event.request.session[1].challengeName == 'PASSWORD_VERIFIER' && event.request.session[1].challengeResult == true) {
        event.response.issueTokens = false;
        event.response.failAuthentication = false;
        event.response.challengeName = 'CUSTOM_CHALLENGE';
    } else if (event.request.session.length == 3 && event.request.session[2].challengeName == 'CUSTOM_CHALLENGE' && event.request.session[2].challengeResult == true) {
        event.response.issueTokens = true;
        event.response.failAuthentication = false;
    } else {
        event.response.issueTokens = false;
        event.response.failAuthentication = true;
    }

    // Return to Amazon Cognito
    callback(null, event);
}
```

------