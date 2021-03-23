# Importing Users into User Pools With a User Migration Lambda Trigger<a name="cognito-user-pools-import-using-lambda"></a>

This approach enables seamless migration of users from your existing user directory to user pools when they use your new Amazon Cognito\-enabled app for the first time, either during their first sign\-in or during the forgot\-password process\. This migration is enabled by a user migration Lambda function which you need to configure in your user pool\. For details on this Lambda trigger including request and response parameters, and example code see [Migrate User Lambda Trigger Parameters](user-pool-lambda-migrate-user.md#cognito-user-pools-lambda-trigger-syntax-user-migration)\.

Before starting the user migration process, create a user migration Lambda function in your AWS account, and configure your user pool to use this Lambda function ARN for the user migration trigger\. Add an authorization policy to your Lambda function to enable only the Amazon Cognito service account principal \("cognito\-idp\.amazonaws\.com"\) and your user pool's SourceARN to invoke it, to prevent any other AWS customer's user pool from invoking your Lambda function\. For more information, see [Using Resource\-Based Policies for AWS Lambda \(Lambda Function Policies\)](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)\. 

**Steps during sign\-in**

1. The user opens your app and signs\-in with the native sign\-in UI screens from your app using the Cognito Identity Provider APIs from an [AWS Mobile SDK](https://aws.amazon.com/mobile/resources/), or using the hosted sign\-in UI provided by Amazon Cognito that you leverage with the [Amazon Cognito Auth SDK](https://github.com/aws/amazon-cognito-auth-js)\.

1. Your app sends the username and password to Amazon Cognito\. If your app has a native sign\-in UI and uses the Cognito Identity Provider SDK, your app must use the USER\_PASSWORD\_AUTH flow, in which the SDK sends the password to the server \(your app must not use the default USER\_SRP\_AUTH flow since the SDK does not send the password to the server in the SRP authentication flow\)\. The USER\_PASSWORD\_AUTH flow is enabled by setting AuthenticationDetails\.authenticationType to "USER\_PASSWORD"\.

1. Amazon Cognito checks if the username exists in the user pool, including as an alias for the user's email, phone number, or preferred\_username\. If the user does not exist, Amazon Cognito calls your user migration Lambda function with parameters including the username and password, as detailed in [Migrate User Lambda Trigger Parameters](user-pool-lambda-migrate-user.md#cognito-user-pools-lambda-trigger-syntax-user-migration)\.

1. Your user migration Lambda function should authenticate the user with your existing user directory or user database, by calling your existing sign\-in service, and it should return user attributes to be stored in the user's profile in the user pool\. If you would like users to continue to use their existing passwords, set the attribute `finalUserStatus` = "CONFIRMED" in the Lambda response\. For the required attributes for the response see [Migrate User Lambda Trigger Parameters](user-pool-lambda-migrate-user.md#cognito-user-pools-lambda-trigger-syntax-user-migration)\.
**Important**  
Do not log the entire request event object in your user migration Lambda code \(since that would include the password in the log record sent to the CloudWatch logs\), and ensure you sanitize the logs so that passwords are not logged\.

1. Amazon Cognito creates the user profile in your user pool, and returns tokens to your app client\.

1. Your app client can now proceed with its normal functionality after sign\-in\.

After the user is migrated, we recommend that your native mobile apps use the `USER_SRP_AUTH` flow for sign\-in\. It authenticates the user using the Secure Remote Password \(SRP\) protocol without sending the password across the network, and provides security benefits over the `USER_PASSWORD_AUTH` flow used during migration\.

In case of errors during migration, including client device or network issues, your app will get error responses from the Amazon Cognito user pools API, and the user account might or might not have been created in your user pool\. The user should then attempt sign\-in again, and if that fails repeatedly, then attempt to reset his or her password using the forgot\-password flow in your app\. 

The forgot\-password flow works in a similar way, except that no password is provided to your user migration Lambda function, and your function only looks up the user in your existing user directory, and returns attributes to be stored in your user pool\. Then Amazon Cognito sends the user a reset password code by email or SMS, and the user can then set a new password in your app\. If your app has a native UI, it needs to provide screens for the forgot\-password flow\. If your app uses the Amazon Cognito hosted UI, then we provide the UI screens\.