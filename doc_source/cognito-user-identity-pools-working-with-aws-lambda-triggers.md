# Customizing user pool workflows with Lambda triggers<a name="cognito-user-identity-pools-working-with-aws-lambda-triggers"></a>

You can create a Lambda function and then activate that function during user pool operations such as user sign\-up, confirmation, and sign\-in \(authentication\) with a Lambda trigger\. You can add authentication challenges, migrate users, and customize verification messages\.

The following table summarizes some of the ways you can use Lambda triggers to customize user pool operations:


****  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html)

**Topics**
+ [Important considerations](#important-lambda-considerations)
+ [Adding a user pool Lambda trigger](#cognito-user-identity-pools-working-with-aws-lambda-triggers-using-lambda)
+ [User pool Lambda trigger event](#cognito-user-pools-lambda-trigger-event-parameter-shared)
+ [User pool Lambda trigger common parameters](#cognito-user-pools-lambda-trigger-syntax-shared)
+ [User pool Lambda trigger sources](#cognito-user-identity-pools-working-with-aws-lambda-trigger-sources)
+ [Pre sign\-up Lambda trigger](user-pool-lambda-pre-sign-up.md)
+ [Post confirmation Lambda trigger](user-pool-lambda-post-confirmation.md)
+ [Pre authentication Lambda trigger](user-pool-lambda-pre-authentication.md)
+ [Post authentication Lambda trigger](user-pool-lambda-post-authentication.md)
+ [Custom authentication challenge Lambda triggers](user-pool-lambda-challenge.md)
+ [Pre token generation Lambda trigger](user-pool-lambda-pre-token-generation.md)
+ [Migrate user Lambda trigger](user-pool-lambda-migrate-user.md)
+ [Custom message Lambda trigger](user-pool-lambda-custom-message.md)
+ [Custom sender Lambda triggers](user-pool-lambda-custom-sender-triggers.md)

## Important considerations<a name="important-lambda-considerations"></a>

Before you work with Lambda functions, consider the following:
+ Except for Custom Sender Lambda triggers, Amazon Cognito invokes Lambda functions synchronously\. When Amazon Cognito calls your Lambda function, it must respond within 5 seconds\. If it does not, Amazon Cognito retries the call\. After three unsuccessful attempts, the function times out\. You can't change this five\-second timeout value\. For more information, see the [Lambda programming model](https://docs.aws.amazon.com/lambda/latest/dg/foundation-progmodel.html)\.
+ If you delete a Lambda trigger, you must update the corresponding trigger in the user pool\. For example, if you delete the post authentication trigger, you must set the **Post authentication** trigger in the corresponding user pool to **none**\. 
+ If your users use the Amazon Cognito hosted UI, they can see errors that Lambda triggers generate in query parameters that Amazon Cognito adds to the Callback URL, except for Custom Sender Lambda triggers\. We recommend that Lambda triggers only generate errors that you want your users to see\. Log any sensitive or debugging information in the Lambda trigger itself\. 
+ When you create a Lambda trigger outside of the Amazon Cognito console, you must add permissions to the Lambda function\. When you add permissions, Amazon Cognito can invoke the function on behalf of your user pool\. You can [add permissions from the Lambda Console](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html) or use the Lambda [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) operation\.

  **Example Lambda Resource\-Based Policy**

  The following example Lambda resource\-based policy grants Amazon Cognito a limited ability to invoke a Lambda function\. Amazon Cognito can only invoke the function when it does so on behalf of both the user pool in the `aws:SourceArn` condition and the account in the `aws:SourceAccount` condition\.

  ```
  {
      "Version": "2012-10-17",
      "Id": "default",
      "Statement": [
          {
              "Sid": "lambda-allow-cognito",
              "Effect": "Allow",
              "Principal": {
                  "Service": "cognito-idp.amazonaws.com"
              },
              "Action": "lambda:InvokeFunction",
              "Resource": "<your Lambda function ARN>",
              "Condition": {
                  "StringEquals": {
                      "AWS:SourceAccount": "<your account number>"
                  },
                  "ArnLike": {
                      "AWS:SourceArn": "<your user pool ARN>"
                  }
              }
          }
      ]
  }
  ```

### Lambda triggers for federated users<a name="lambda-triggers-for-federated-users"></a>

You can use the following Lambda triggers to customize your user pool workflows for users who sign in with a federated provider\. 

**Note**  
Federated users must use the Amazon Cognito hosted UI to sign in\.
For some actions, federated users invoke more than one Lambda function\.


**Federated user trigger sources**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Pre sign\-up | PreSignUp\_ExternalProvider | A federated user signs in from the Amazon Cognito hosted UI sign\-in page for the first time\. | 
| Post confirmation | PostConfirmation\_ConfirmSignUp | A federated user signs in from the Amazon Cognito hosted UI sign\-in page for the first time\. | 
| Pre authentication | PreAuthentication\_Authentication | A federated user signs in from the Amazon Cognito hosted UI sign\-in page, except at first sign\-in\. | 
| Post authentication | PostAuthentication\_Authentication | A federated user successfully authenticates from the Amazon Cognito hosted UI sign\-in page, except at first sign\-in\. | 
| Pre token generation | TokenGeneration\_HostedAuth | A federated user successfully authenticates from the Amazon Cognito hosted UI sign\-in page and is about to be issued tokens, except at first sign\-in\. | 

Federated sign\-in does not invoke any [Custom authentication challenge Lambda triggers](user-pool-lambda-challenge.md), [Migrate user Lambda trigger](user-pool-lambda-migrate-user.md), [Custom message Lambda trigger](user-pool-lambda-custom-message.md), or [Custom sender Lambda triggers](user-pool-lambda-custom-sender-triggers.md) in your user pool\.

## Adding a user pool Lambda trigger<a name="cognito-user-identity-pools-working-with-aws-lambda-triggers-using-lambda"></a>

------
#### [ Original console ]

**To add a user pool Lambda trigger with the console**

1. Use the [Lambda console](https://console.aws.amazon.com/lambda/home) to create a Lambda function \. For more information on Lambda functions, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\.

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. Then choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. In your user pool, choose the **Triggers** tab from the navigation bar\.

1. Choose a Lambda trigger, such as **Pre sign\-up** or **Pre authentication**\. Then choose your Lambda function from the **Lambda function** drop\-down list\.

1. Choose **Save changes**\.

1. You can use Amazon CloudWatch in the Lambda console to log your Lambda function\. For more information, see [Accessing Amazon CloudWatch Logs for Lambda](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-logs.html)\.

------
#### [ New console ]

**To add a user pool Lambda trigger with the console**

1. Use the [Lambda console](https://console.aws.amazon.com/lambda/home) to create a Lambda function\. For more information on Lambda functions, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\.

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), and then choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **User pool properties** tab and locate **Lambda triggers**\.

1. Choose **Add a Lambda trigger**\.

1. Select a Lambda trigger **Category** based on the stage of authentication that you want to customize\.

1. Select **Assign Lambda function** and select a function in the same AWS Region as your user pool\.
**Note**  
If your AWS Identity and Access Management \(IAM\) credentials have permission to update the Lambda function, Amazon Cognito adds a Lambda resource\-based policy\. With this policy, Amazon Cognito can invoke the function that you select\. If the signed\-in credentials do not have sufficient IAM permissions, you must update the resource\-based policy separately\. For more information, see [Important considerations](#important-lambda-considerations)\.

1. Choose **Save changes**\.

1. You can use CloudWatch in the Lambda console to log your Lambda function \. For more information, see [Accessing CloudWatch Logs for Lambda](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-logs.html)\.

------

## User pool Lambda trigger event<a name="cognito-user-pools-lambda-trigger-event-parameter-shared"></a>

Amazon Cognito passes event information to your Lambda function\. The Lambda function returns the same event object back to Amazon Cognito with any changes in the response\. This event shows the Lambda trigger common parameters:

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

## User pool Lambda trigger common parameters<a name="cognito-user-pools-lambda-trigger-syntax-shared"></a>

**version**  
The version number of your Lambda function\.

**triggerSource**  
The name of the event that triggered the Lambda function\. For a description of each triggerSource see [User pool Lambda trigger sources](#cognito-user-identity-pools-working-with-aws-lambda-trigger-sources)\.

**region**  
The AWS Region as an `AWSRegion` instance\.

**userPoolId**  
The user pool ID for the user pool\.

**userName**  
The user name of the current user\.

**callerContext**  
The caller context\. This context consists of the following:     
**awsSdkVersion**  
The AWS SDK version number\.  
**clientId**  
The user pool app client ID\.

**request**  
The request from the Amazon Cognito service\. This request must include the following:    
**userAttributes**  
One or more pairs of user attribute names and values\. Each pair takes the form "*name*": "*value*"\. 

**response**  
The response from your Lambda trigger\. The return parameters in the response depend on the triggering event\.

## User pool Lambda trigger sources<a name="cognito-user-identity-pools-working-with-aws-lambda-trigger-sources"></a>

This section describes each Amazon Cognito Lambda triggerSource parameter and its triggering event\. 


**Sign\-up, confirmation, and sign\-in \(authentication\) triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Pre sign\-up | PreSignUp\_SignUp | Pre sign\-up\. | 
| Pre sign\-up | PreSignUp\_AdminCreateUser | Pre sign\-up when an admin creates a new user\. | 
| Pre sign\-up | PreSignUp\_ExternalProvider | Pre sign\-up for external identity providers\. | 
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
| Pre token generation | TokenGeneration\_HostedAuth |  Amazon Cognito authenticates the user from your hosted UI sign\-in page\. | 
| Pre token generation | TokenGeneration\_Authentication | User authentication flows complete\. | 
| Pre token generation | TokenGeneration\_NewPasswordChallenge | Admin creates the user\. Amazon Cognito invokes this when the user must change a temporary password\. | 
| Pre token generation | TokenGeneration\_AuthenticateDevice | End of the authentication of a user device\. | 
| Pre token generation | TokenGeneration\_RefreshTokens | User tries to refresh the identity and access tokens\. | 


**Migrate user triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| User migration | UserMigration\_Authentication | User migration at the time of sign\-in\. | 
| User migration | UserMigration\_ForgotPassword | User migration during the forgot\-password flow\. | 


**Custom message triggers**  

| Trigger | triggerSource value | Triggering event | 
| --- | --- | --- | 
| Custom message | CustomMessage\_SignUp | Custom message when a user signs up in your user pool\. | 
| Custom message | CustomMessage\_AdminCreateUser | Custom message when you create a user as an administrator and Amazon Cognito sends them a temporary password\. | 
| Custom message | CustomMessage\_ResendCode | Custom message when your existing user requests a new confirmation code\. | 
| Custom message | CustomMessage\_ForgotPassword | Custom message when your user requests a password reset\. | 
| Custom message | CustomMessage\_UpdateUserAttribute | Custom message when a user changes their email address or phone number and Amazon Cognito sends a verification code\. | 
| Custom message | CustomMessage\_VerifyUserAttribute | Custom message when a user adds an email address or phone number and Amazon Cognito sends a verification code\. | 
| Custom message | CustomMessage\_Authentication | Custom message when a user who has configured SMS MFA signs in\. | 