# User Pool Authentication Flow<a name="amazon-cognito-user-pools-authentication-flow"></a>

Modern authentication flows incorporate new challenge types, in addition to a password, to verify the identity of users\. We generalize authentication into two common steps, which are implemented through two API operations: `InitiateAuth` and `RespondToAuthChallenge`\.

A user authenticates by answering successive challenges until authentication either fails or the user is issued tokens\. With these two steps, which can be repeated to include different challenges, we can support any custom authentication flow\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can customize your authentication flow with AWS Lambda triggers\. These triggers issue and verify their own challenges as part of the authentication flow\.

You can also use admin authentication flow for use on secure backend servers\. You can use the user migration authentication flow to allow user migration without requiring your users to reset their passwords\.

**Note**  
We allow five failed sign\-in attempts\. After that we start temporary lockouts with exponentially increasing times starting at 1 second and doubling after each failed attempt up to about 15 minutes\. Attempts during a temporary lockout period are ignored\. After the temporary lockout period, if the next attempt fails, a new temporary lockout starts with twice the duration as the last\. Waiting about 15 minutes without any attempts will also reset the temporary lockout\. Please note that this behavior is subject to change\.

**Topics**
+ [Client\-Side Authentication Flow](#amazon-cognito-user-pools-client-side-authentication-flow)
+ [Server\-Side Authentication Flow](#amazon-cognito-user-pools-server-side-authentication-flow)
+ [Custom Authentication Flow](#amazon-cognito-user-pools-custom-authentication-flow)
+ [Built\-in Authentication Flow and Challenges](#Built-in-authentication-flow-and-challenges)
+ [Custom Authentication Flow and Challenges](#Custom-authentication-flow-and-challenges)
+ [Use SRP Password Verification in Custom Authentication Flow](#Using-SRP-password-verification-in-custom-authentication-flow)
+ [Admin authentication flow](#amazon-cognito-user-pools-admin-authentication-flow)
+ [User migration authentication flow](#amazon-cognito-user-pools-user-migration-authentication-flow)

## Client\-Side Authentication Flow<a name="amazon-cognito-user-pools-client-side-authentication-flow"></a>

Here's how user pool authentication works for end\-user client\-side apps created with the [AWS Mobile SDK for Android, AWS Mobile SDK for iOS, or AWS SDK for JavaScript](https://aws.amazon.com/mobile/resources/):

1. The user enters their user name and password into the app\.

1. The app calls the `InitiateAuth` operation with the user's user name and SRP details\.

   This method returns the authentication parameters\.
**Note**  
The app generates the SRP details by using the Amazon Cognito SRP support in the Android, iOS, and JavaScript SDKs\.

1. The app calls the `RespondToAuthChallenge` operation\. If the call succeeds, it returns the user's tokens, and the authentication flow is complete\.

   If another challenge is required, no tokens are returned\. Instead, the call to `RespondToAuthChallenge` returns a session\.

1. If `RespondToAuthChallenge` returns a session, the app calls `RespondToAuthChallenge` again, this time with the session and the challenge response \(for example, MFA code\)\.

## Server\-Side Authentication Flow<a name="amazon-cognito-user-pools-server-side-authentication-flow"></a>

If you don't have an end\-user app, but instead you're using a Java, Ruby, or Node\.js secure backend or server\-side app, you can use the authenticated server\-side API for Amazon Cognito user pools\.

For server\-side apps, user pool authentication is similar to that for client\-side apps, except for the following:
+ The server\-side app calls the `AdminInitiateAuth` API operation \(instead of `InitiateAuth`\)\. This operation requires AWS admin credentials\. This operation returns the authentication parameters\.
+ Once it has the authentication parameters, the app calls the `AdminRespondToAuthChallenge` API operation \(instead of `RespondToAuthChallenge`\), which also requires AWS admin credentials\.

The `AdminInitiateAuth` and `AdminRespondToAuthChallenge` operations can't accept user\-name\-and\-password user credentials for admin sign\-in, unless you explicitly enable them to do so by doing one of the following:
+ Pass `ADMIN_USER_PASSWORD_AUTH` \(formerly known as `ADMIN_NO_SRP_AUTH`\) for the `ExplicitAuthFlow` parameter in your server\-side app's call to `CreateUserPoolClient` or `UpdateUserPoolClient`\.
+ Choose `Enable sign-in API for server-based authentication (ADMIN_USER_PASSWORD_AUTH)` in the **App clients** tab in **Create a user pool**\. For more information, see [Configuring a User Pool App Client](user-pool-settings-client-apps.md)\.

## Custom Authentication Flow<a name="amazon-cognito-user-pools-custom-authentication-flow"></a>

Amazon Cognito user pools also enable custom authentication flows, which can help you create a challenge/response\-based authentication model using AWS Lambda triggers\.

The custom authentication flow is designed to allow for a series of challenge and response cycles that can be customized to meet different requirements\. The flow starts with a call to the `InitiateAuth` API operation that indicates the type of authentication that will be used and provides any initial authentication parameters\. Amazon Cognito will respond to the `InitiateAuth` call with one of the following: 
+ A challenge for the user along with a session and parameters\.
+ An error if the user fails to authenticate\.
+ ID, access, and refresh tokens, if the supplied parameters in the `InitiateAuth` call are sufficient to sign the user in\. \(Typically a challenge needs to be answered first, but your custom code needs to decide this\.\)

 If Amazon Cognito responds to the `InitiateAuth` call with a challenge, the app gathers more input and calls the `RespondToAuthChallenge` operation, providing the challenge responses and passing back the session\. Amazon Cognito responds to the `RespondToAuthChallenge` call similarly to the `InitiateAuth` call, providing tokens if the user is signed in, another challenge, or an error\. If another challenge is returned, the sequence repeats with the app calling `RespondToAuthChallenge` until the user is signed in or an error is returned\. More details are provided in the API documentation for the `InitiateAuth` and `RespondToAuthChallenge` API operations\. 

## Built\-in Authentication Flow and Challenges<a name="Built-in-authentication-flow-and-challenges"></a>

Amazon Cognito has some built\-in `AuthFlow` and `ChallengeName` values for a standard authentication flow to validate user name and password through the Secure Remote Password \(SRP\) protocol\. This flow is built into the iOS, Android, and JavaScript SDKs for Amazon Cognito\. At a high level, the flow starts by sending `USER_SRP_AUTH` as the `AuthFlow` to `InitiateAuth` along with `USERNAME` and `SRP_A` values in `AuthParameters`\. If the `InitiateAuth` call is successful, the response included `PASSWORD_VERIFIER` as the `ChallengeName` and `SRP_B` in the challenge parameters\. The app then calls `RespondToAuthChallenge` with the `PASSWORD_VERIFIER` `ChallengeName` and the necessary parameters in `ChallengeResponses`\. If the call to `RespondToAuthChallenge` is successful and the user is signed in, the tokens are returned\. If multi\-factor authentication \(MFA\) is enabled for the user, a `ChallengeName` of `SMS_MFA` is returned, and the app can provide the necessary code through another call to `RespondToAuthChallenge`\. 

## Custom Authentication Flow and Challenges<a name="Custom-authentication-flow-and-challenges"></a>

An app can initiate a custom authentication flow by calling `InitiateAuth` with `CUSTOM_AUTH` as the `Authflow`\. With a custom authentication flow, the challenges and verification of the responses are controlled through three AWS Lambda triggers\. 
+ The `DefineAuthChallenge` Lambda trigger takes as input a session array of previous challenges and responses\. It then outputs the next challenge name and Booleans that indicate whether the user is authenticated \(and should be granted tokens\) or not\. This Lambda trigger is a state machine that controls the user’s path through the challenges\. 
+ The `CreateAuthChallenge` Lambda trigger takes a challenge name as input and generates the challenge and parameters to evaluate the response\. `CreateAuthChallenge` is called when `DefineAuthChallenge` returns `CUSTOM_CHALLENGE` as the next challenge, and the next type of challenge is passed in the challenge metadata parameter\. 
+ The `VerifyAuthChallengeResponse` Lambda function evaluates the response and returns a Boolean to indicate if the response was valid\. 

A custom authentication flow can also use a combination of built\-in challenges such as SRP password verification and MFA through SMS\. It can use custom challenges such as CAPTCHA or secret questions\. 

## Use SRP Password Verification in Custom Authentication Flow<a name="Using-SRP-password-verification-in-custom-authentication-flow"></a>

If you want to include SRP in a custom authentication flow, you need to start with it\.
+ To initiate SRP password verification in a custom flow, the app calls `InitiateAuth` with `CUSTOM_AUTH` as the `Authflow`\. It includes in the `AuthParameters` map `SRP_A:` \(the SRP A value\) and `CHALLENGE_NAME: SRP_A`\.
+ The `DefineAuthChallenge` Lambda trigger is invoked with an initial session of `challengeName: SRP_A` and `challengeResult: true`, and should respond with `challengeName: PASSWORD_VERIFIER`, `issueTokens: false`, and `failAuthentication: false`\.
+  The app next needs to call `RespondToAuthChallenge` with `challengeName: PASSWORD_VERIFIER` and the other parameters required for SRP in the `challengeResponses` map\. 
+ If the password is verified, the `DefineAuthChallenge` Lambda trigger is invoked with a second session of `challengeName: PASSWORD_VERIFIER` and `challengeResult: true`\. At that point the `DefineAuthChallenge` Lambda trigger can respond with `challengeName: CUSTOM_CHALLENGE` to start the custom challenge\.
+ If MFA is enabled for a user, it is handled automatically after the password is verified\. 

**Note**  
The Amazon Cognito hosted sign\-in webpage does not support the custom authentication flow\.

For more information about the Lambda triggers, including sample code, see [Customizing User Pool Workflows with Lambda Triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.

**Note**  
The Amazon Cognito hosted sign\-in web page does not support the custom authentication flow\.

## Admin authentication flow<a name="amazon-cognito-user-pools-admin-authentication-flow"></a>

The API operations described [Custom Authentication Flow](#amazon-cognito-user-pools-custom-authentication-flow) with the use of SRP for password verification is the recommended approach for authentication\. The iOS, Android, and JavaScript SDKs are based on that approach and make it easy to use SRP\. However, there is an alternative set of admin API operations designed for use on secure backend servers if you want to avoid the SRP calculations\. For these backend admin implementations, `AdminInitiateAuth` is used in place of `InitiateAuth`, and `AdminRespondToAuthChallenge` is used in place of `RespondToAuthChallenge`\. When using these operations, the password can be submitted as plaintext so the SRP calculations are not needed\. Here is an example:

```
AdminInitiateAuth Request {
    "AuthFlow":"ADMIN_USER_PASSWORD_AUTH",
    "AuthParameters":{
        "USERNAME":"<username>",
        "PASSWORD":"<password>"
        },
    "ClientId":"<clientId>",
    "UserPoolId":"<userPoolId>"
}
```

These admin authentication operations require developer credentials and use the AWS Signature Version 4 \(SigV4\) signing process\. These operations are available in standard AWS SDKs including Node\.js, which is convenient for use in Lambda functions\. In order to use these operations and have them accept passwords in plaintext, you must enable them for the app in the console\. Alternatively, you can pass `ADMIN_USER_PASSWORD_AUTH` for the `ExplicitAuthFlow` parameter in calls to `CreateUserPoolClient` or `UpdateUserPoolClient`\. The `ADMIN_USER_PASSWORD_AUTH` `AuthFlow` is not accepted for the `InitiateAuth` and `RespondToAuthChallenge` operations\.

In the `AdminInitiateAuth` response `ChallengeParameters`, the `USER_ID_FOR_SRP` attribute, if present, will contain the user's actual user name, not an alias \(such as email address or phone number\)\. In your call to `AdminRespondToAuthChallenge`, in the `ChallengeResponses`, you must pass this user name in the `USERNAME` parameter\. 

**Note**  
Because it's designed for backend admin implementations, admin authentication flow doesn't support device tracking\. When device tracking is enabled, admin authentication succeeds, but any call to refresh the access token will fail\.

## User migration authentication flow<a name="amazon-cognito-user-pools-user-migration-authentication-flow"></a>

A user migration Lambda trigger allows easy migration of users from a legacy user management system into your user pool\. To avoid making your users reset their passwords during user migration, choose the `USER_PASSWORD_AUTH` authentication flow\. This flow sends your users' passwords to the service over an encrypted SSL connection during authentication\.

When you have completed migrating all your users, we recommend switching flows to the more secure SRP flow\. The SRP flow does not send any passwords over the network\.

To learn more about Lambda triggers see [Customizing User Pool Workflows with Lambda Triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.

For more information about migrating users with a Lambda trigger see [Importing Users into User Pools With a User Migration Lambda Trigger](cognito-user-pools-import-using-lambda.md)\.