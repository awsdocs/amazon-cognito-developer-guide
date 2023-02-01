# Define Auth challenge Lambda trigger<a name="user-pool-lambda-define-auth-challenge"></a>

![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Define auth challenge**  
 Amazon Cognito invokes this trigger to initiate the [custom authentication flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html#amazon-cognito-user-pools-custom-authentication-flow)\.

The request for this Lambda trigger contains `session`\. The `session` parameter is an array that contains all of the challenges that are presented to the user in the current authentication process\. The request also includes the corresponding result\. The `session` array stores challenge details \(`ChallengeResult`\) in chronological order\. The challenge `session[0]` represents the first challenge that the user receives\.

 You can have Amazon Cognito verify user passwords before it issues your custom challenges\. Here is an overview of the process:

1. Your app initiates sign\-in by calling `InitiateAuth` or `AdminInitiateAuth` with the `AuthParameters` map\. Parameters must include `CHALLENGE_NAME: SRP_A,` and values for `SRP_A` and `USERNAME`\. 

1. Amazon Cognito invokes your define auth challenge Lambda trigger with an initial session that contains `challengeName: SRP_A` and `challengeResult: true`\. 

1. After receiving those inputs, your Lambda function responds with `challengeName: PASSWORD_VERIFIER`, `issueTokens: false`, `failAuthentication: false`\. 

1. If the password verification succeeds, Amazon Cognito invokes your Lambda function again with a new session containing `challengeName: PASSWORD_VERIFIER` and `challengeResult: true`\. 

1. To initiate your custom challenges, your Lambda function responds with `challengeName: CUSTOM_CHALLENGE`, `issueTokens: false`, and `failAuthentication: false`\. If you don't want to start your custom auth flow with password verification, you can initiate sign\-in with the `AuthParameters` map including `CHALLENGE_NAME: CUSTOM_CHALLENGE`\. 

1. The challenge loop repeats until all challenges are answered\.

**Topics**
+ [Define Auth challenge Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-define-auth-challenge)
+ [Define Auth challenge example](#aws-lambda-triggers-define-auth-challenge-example)

## Define Auth challenge Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-define-auth-challenge"></a>

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
}
```

------

### Define Auth challenge request parameters<a name="cognito-user-pools-lambda-trigger-syntax-define-auth-challenge-request"></a>

 When Amazon Cognito invokes your Lambda function, Amazon Cognito provides the following parameters:

**userAttributes**  
One or more name\-value pairs that represent user attributes\.

**userNotFound**  
A Boolean that Amazon Cognito populates when `PreventUserExistenceErrors` is set to `ENABLED` for your user pool client\. A value of `true` means that the user id \(user name, email address, etc\.\) did not match any existing users\. When `PreventUserExistenceErrors` is set to `ENABLED`, the service doesn't inform the app of nonexistent users\. We recommend that your Lambda functions maintain the same user experience and account for latency\. This way, the caller can't detect different behavior when the user exists or doesn’t exist\.

**session**  
An array of `ChallengeResult` elements\. Each contains the following elements:    
**challengeName**  
One of the following challenge types: `CUSTOM_CHALLENGE`, `SRP_A`, `PASSWORD_VERIFIER`, `SMS_MFA, DEVICE_SRP_AUTH`, `DEVICE_PASSWORD_VERIFIER`, or `ADMIN_NO_SRP_AUTH`\.   
When your function determines if a user has successfully authenticated and you should issue them tokens, always check `challengeName` in your `DefineAuthChallenge` Lambda trigger to make sure it matches the expected value\.  
**challengeResult**  
Set to `true` if the user successfully completed the challenge, or `false` otherwise\.  
**challengeMetadata**  
Your name for the custom challenge\. Used only if `challengeName` is `CUSTOM_CHALLENGE`\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the define auth challenge trigger\. To pass this data to your Lambda function, you can use the `ClientMetadata` parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API operations\. The request that invokes the define auth challenge function doesn't include data passed in the ClientMetadata parameter in [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) and [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API operations\.

### Define Auth challenge response parameters<a name="cognito-user-pools-lambda-trigger-syntax-define-auth-challenge-response"></a>

In the response, you can return the next stage of the authentication process\.

**challengeName**  
A string that contains the name of the next challenge\. If you want to present a new challenge to your user, specify the challenge name here\.

**issueTokens**  
If you determine that the user has completed the authentication challenges sufficiently, set to `true`\. If the user has not met the challenges sufficiently, set to `false`\.

**failAuthentication**  
If you want to end the current authentication process, set to `true`\. To continue the current authentication process, set to `false`\.

## Define Auth challenge example<a name="aws-lambda-triggers-define-auth-challenge-example"></a>

This example defines a series of challenges for authentication and issues tokens only if the user has completed all of the challenges successfully\.

------
#### [ Node\.js ]

```
const handler = async (event) => {
  if (
    event.request.session.length == 1 &&
    event.request.session[0].challengeName == "SRP_A"
  ) {
    event.response.issueTokens = false;
    event.response.failAuthentication = false;
    event.response.challengeName = "PASSWORD_VERIFIER";
  } else if (
    event.request.session.length == 2 &&
    event.request.session[1].challengeName == "PASSWORD_VERIFIER" &&
    event.request.session[1].challengeResult == true
  ) {
    event.response.issueTokens = false;
    event.response.failAuthentication = false;
    event.response.challengeName = "CUSTOM_CHALLENGE";
  } else if (
    event.request.session.length == 3 &&
    event.request.session[2].challengeName == "CUSTOM_CHALLENGE" &&
    event.request.session[2].challengeResult == true
  ) {
    event.response.issueTokens = false;
    event.response.failAuthentication = false;
    event.response.challengeName = "CUSTOM_CHALLENGE";
  } else if (
    event.request.session.length == 4 &&
    event.request.session[3].challengeName == "CUSTOM_CHALLENGE" &&
    event.request.session[3].challengeResult == true
  ) {
    event.response.issueTokens = true;
    event.response.failAuthentication = false;
  } else {
    event.response.issueTokens = false;
    event.response.failAuthentication = true;
  }

  return event;
};

export { handler }
```

------