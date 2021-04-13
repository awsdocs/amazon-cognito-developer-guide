# Customizing User Pool Workflows with Lambda Triggers<a name="cognito-user-identity-pools-working-with-aws-lambda-triggers"></a>

You can create an AWS Lambda function and then trigger that function during user pool operations such as user sign\-up, confirmation, and sign\-in \(authentication\) with a Lambda trigger\. You can add authentication challenges, migrate users, and customize verification messages\.

The following table summarizes some of the customizations that can be made:


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html)

**Topics**
+ [Important Considerations](#important-lambda-considerations)
+ [Adding a User Pool Lambda Trigger](#cognito-user-identity-pools-working-with-aws-lambda-triggers-using-lambda)
+ [User Pool Lambda Trigger Event](#cognito-user-pools-lambda-trigger-event-parameter-shared)
+ [User Pool Lambda Trigger Common Parameters](#cognito-user-pools-lambda-trigger-syntax-shared)
+ [User Pool Lambda Trigger Sources](#cognito-user-identity-pools-working-with-aws-lambda-trigger-sources)
+ [Pre Sign\-up Lambda Trigger](user-pool-lambda-pre-sign-up.md)
+ [Post Confirmation Lambda Trigger](user-pool-lambda-post-confirmation.md)
+ [Pre Authentication Lambda Trigger](user-pool-lambda-pre-authentication.md)
+ [Post Authentication Lambda Trigger](user-pool-lambda-post-authentication.md)
+ [Custom Authentication Challenge Lambda Triggers](user-pool-lambda-challenge.md)
+ [Pre Token Generation Lambda Trigger](user-pool-lambda-pre-token-generation.md)
+ [Migrate User Lambda Trigger](user-pool-lambda-migrate-user.md)
+ [Custom Message Lambda Trigger](user-pool-lambda-custom-message.md)
+ [Custom sender Lambda Triggers](user-pool-lambda-custom-sender-triggers.md)

## Important Considerations<a name="important-lambda-considerations"></a>

The following information is important to consider before you start working with Lambda functions:
+ Except for Custom Sender Lambda triggers, Amazon Cognito invokes Lambda functions synchronously\. When called, your Lambda function must respond within 5 seconds\. If it does not, Amazon Cognito retries the call\. After 3 unsuccessful attempts, the function times out\. This 5\-second timeout value cannot be changed\. For more information see the [Lambda programming model](https://docs.aws.amazon.com/lambda/latest/dg/programming-model-v2.html)\.
+ If you delete an AWS Lambda trigger, you must update the corresponding trigger in the user pool\. For example, if you delete the post authentication trigger, you must set the **Post authentication** trigger in the corresponding user pool to **none**\. 
+ Except for Custom Sender Lambda triggers, errors thrown by Lambda triggers will be visible directly to your end users if they are using Amazon Cognito Hosted UI as query parameters in the Callback URL\. As a recommended best practice, end user facing errors should be thrown from the Lambda triggers and any sensitive or debugging information should be logged in the Lambda trigger itself\. 

## Adding a User Pool Lambda Trigger<a name="cognito-user-identity-pools-working-with-aws-lambda-triggers-using-lambda"></a>

**To add a user pool Lambda trigger with the console**

1. Create a Lambda function using the [Lambda console](https://console.aws.amazon.com/lambda/home)\. For more information on Lambda functions, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\.

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. In your user pool, choose the **Triggers** tab from the navigation bar\.

1. Choose a Lambda trigger such as **Pre sign\-up** or **Pre authentication** and choose your Lambda function from the **Lambda function** drop\-down list\.

1. Choose **Save changes**\.

1. You can log your Lambda function using CloudWatch in the Lambda console\. For more information see [Accessing CloudWatch Logs for Lambda](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-logs.html)\.

## User Pool Lambda Trigger Event<a name="cognito-user-pools-lambda-trigger-event-parameter-shared"></a>

Amazon Cognito passes event information to your Lambda function which returns the same event object back to Amazon Cognito with any changes in the response\. This event shows the Lambda trigger common parameters:

------
#### [ JSON ]

```
{
    "version": "string",
    "triggerSource": "string",
    "region": AWSRegion,
    "userPoolId": "string",
    "userName": "string",
    "callerContext": 
        {
            "awsSdkVersion": "string",
            "clientId": "string"
        },
    "request":
        {
            "userAttributes": {
                "string": "string",
                ....
            }
        },
    "response": {}
}
```

------

## User Pool Lambda Trigger Common Parameters<a name="cognito-user-pools-lambda-trigger-syntax-shared"></a>

**version**  
The version number of your Lambda function\.

**triggerSource**  
The name of the event that triggered the Lambda function\. For a description of each triggerSource see [User Pool Lambda Trigger Sources](#cognito-user-identity-pools-working-with-aws-lambda-trigger-sources)\.

**region**  
The AWS Region, as an `AWSRegion` instance\.

**userPoolId**  
The user pool ID for the user pool\.

**userName**  
The username of the current user\.

**callerContext**  
The caller context, which consists of the following:     
**awsSdkVersion**  
The AWS SDK version number\.  
**clientId**  
The ID of the client associated with the user pool\.

**request**  
The request from the Amazon Cognito service\. This request must include:     
**userAttributes**  
One or more pairs of user attribute names and values\. Each pair is in the form "*name*": "*value*"\. 

**response**  
The response from your Lambda trigger\. The return parameters in the response depend on the triggering event\.

## User Pool Lambda Trigger Sources<a name="cognito-user-identity-pools-working-with-aws-lambda-trigger-sources"></a>

This section describes each Amazon Cognito Lambda triggerSource parameter, and its triggering event\. 


**Sign\-up, confirmation, and sign\-in \(authentication\) triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Pre sign\-up | PreSignUp\_SignUp | Pre sign\-up\. | 
| Pre sign\-up | PreSignUp\_AdminCreateUser | Pre sign\-up when an admin creates a new user\. | 
| Pre sign\-up | PreSignUp\_ExternalProvider | Pre sign\-up when external user creates a new user\. | 
| Post confirmation | PostConfirmation\_ConfirmSignUp | Post sign\-up confirmation\. | 
| Post confirmation | PostConfirmation\_ConfirmForgotPassword | Post Forgot Password confirmation\. | 
| Pre authentication | PreAuthentication\_Authentication | Pre authentication\. | 
| Post authentication | PostAuthentication\_Authentication | Post authentication\. | 


**Custom authentication challenge triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Define auth challenge | DefineAuthChallenge\_Authentication | Define Auth Challenge\. | 
| Create auth challenge | CreateAuthChallenge\_Authentication | Create Auth Challenge\. | 
| Verify auth challenge | VerifyAuthChallengeResponse\_Authentication | Verify Auth Challenge Response\. | 


**Pre token generation triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Pre token generation | TokenGeneration\_HostedAuth | Called during authentication from the Amazon Cognito hosted UI sign\-in page\. | 
| Pre token generation | TokenGeneration\_Authentication | Called after user authentication flows have completed\. | 
| Pre token generation | TokenGeneration\_NewPasswordChallenge | Called after the user is created by an admin\. This flow is invoked when the user has to change a temporary password\. | 
| Pre token generation | TokenGeneration\_AuthenticateDevice | Called at the end of the authentication of a user device\. | 
| Pre token generation | TokenGeneration\_RefreshTokens | Called when a user tries to refresh the identity and access tokens\. | 


**Migrate user triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| User migration | UserMigration\_Authentication | User migration at the time of sign in\. | 
| User migration | UserMigration\_ForgotPassword | User migration during the forgot\-password flow\. | 


**Custom message triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Custom message | CustomMessage\_SignUp | Custom message – To send the confirmation code post sign\-up\. | 
| Custom message | CustomMessage\_AdminCreateUser | Custom message – To send the temporary password to a new user\. | 
| Custom message | CustomMessage\_ResendCode | Custom message – To resend the confirmation code to an existing user\. | 
| Custom message | CustomMessage\_ForgotPassword | Custom message – To send the confirmation code for Forgot Password request\. | 
| Custom message | CustomMessage\_UpdateUserAttribute | Custom message – When a user's email or phone number is changed, this trigger sends a verification code automatically to the user\. Cannot be used for other attributes\. | 
| Custom message | CustomMessage\_VerifyUserAttribute | Custom message – This trigger sends a verification code to the user when they manually request it for a new email or phone number\. | 
| Custom message | CustomMessage\_Authentication | Custom message – To send MFA code during authentication\. | 
