# Custom authentication challenge Lambda triggers<a name="user-pool-lambda-challenge"></a>

![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Challenge Lambda triggers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

These Lambda triggers issue and verify their own challenges as part of a user pool [custom authentication flow](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-authentication-flow.html#amazon-cognito-user-pools-custom-authentication-flow)\.

**Define auth challenge**  
Amazon Cognito invokes this trigger to initiate the custom authentication flow\.

**Create auth challenge**  
Amazon Cognito invokes this trigger after **Define Auth Challenge** to create a custom challenge\.

**Verify auth challenge response**  
Amazon Cognito invokes this trigger to verify if the response from the end user for a custom challenge is valid or not\.

You can incorporate new challenge types with these challenge Lambda triggers\. For example, these challenge types might include CAPTCHAs or dynamic challenge questions\.

You can generalize authentication into two common steps with the user pool InitiateAuth and RespondToAuthChallenge API methods\.

In this flow, a user authenticates by answering successive challenges until authentication either fails or the user is issued tokens\. These two API calls can be repeated to include different challenges\.

**Note**  
The Amazon Cognito hosted sign\-in web page does not support the custom authentication flow\.

**Topics**
+ [Define Auth challenge Lambda trigger](user-pool-lambda-define-auth-challenge.md)
+ [Create Auth challenge Lambda trigger](user-pool-lambda-create-auth-challenge.md)
+ [Verify Auth challenge response Lambda trigger](user-pool-lambda-verify-auth-challenge-response.md)