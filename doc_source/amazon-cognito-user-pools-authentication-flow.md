# User pool authentication flow<a name="amazon-cognito-user-pools-authentication-flow"></a>

To verify the identity of users, modern authentication flows incorporate new challenge types, in addition to passwords\. Amazon Cognito authentication typically requires that you implement two API operations in the following order: 

1. `InitiateAuth`

1. `RespondToAuthChallenge`

A user authenticates by answering successive challenges until authentication either fails or Amazon Cognito issues tokens to the user\. You can repeat these steps with Amazon Cognito, in a process that includes different challenges, to support any custom authentication flow\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can use AWS Lambda triggers to customize the way users authenticate\. These triggers issue and verify their own challenges as part of the authentication flow\.

You can also use the admin authentication flow for secure backend servers\. You can use the user migration authentication flow to make user migration possible without the requirement that your users to reset their passwords\.

**Note**  
Users can attempt but fail to sign in correctly five times before Amazon Cognito temporarily locks them out\. Lockout time starts at one second and increases exponentially, doubling after each subsequent failed attempt, up to about 15 minutes\. Amazon Cognito ignores attempts to log in during a temporary lockout period, and these attempts don't initiate a new lockout period\. After a user waits 15 minutes, Amazon Cognito resets the temporary lockout\. This behavior is subject to change\.

**Topics**
+ [Client\-side authentication flow](#amazon-cognito-user-pools-client-side-authentication-flow)
+ [Server\-side authentication flow](#amazon-cognito-user-pools-server-side-authentication-flow)
+ [Custom authentication flow](#amazon-cognito-user-pools-custom-authentication-flow)
+ [Built\-in authentication flow and challenges](#Built-in-authentication-flow-and-challenges)
+ [Custom authentication flow and challenges](#Custom-authentication-flow-and-challenges)
+ [Use SRP password verification in custom authentication flow](#Using-SRP-password-verification-in-custom-authentication-flow)
+ [Admin authentication flow](#amazon-cognito-user-pools-admin-authentication-flow)
+ [User migration authentication flow](#amazon-cognito-user-pools-user-migration-authentication-flow)

## Client\-side authentication flow<a name="amazon-cognito-user-pools-client-side-authentication-flow"></a>

The following process works for user client\-side apps that you create with the [AWS Mobile SDK for Android, AWS Mobile SDK for iOS, or AWS SDK for JavaScript](https://aws.amazon.com/mobile/resources/):

1. The user enters their user name and password into the app\.

1. The app calls the `InitiateAuth` operation with the user's user name and Secure Remote Password \(SRP\) details\.

   This API operation returns the authentication parameters\.
**Note**  
The app generates SRP details with the Amazon Cognito SRP features that are built in to AWS SDKs\.

1. The app calls the `RespondToAuthChallenge` operation\. If the call succeeds, Amazon Cognito returns the user's tokens, and the authentication flow is complete\.

   If Amazon Cognito requires another challenge, the call to `RespondToAuthChallenge` returns no tokens\. Instead, the call returns a session\.

1. If `RespondToAuthChallenge` returns a session, the app calls `RespondToAuthChallenge` again, this time with the session and the challenge response \(for example, MFA code\)\.

## Server\-side authentication flow<a name="amazon-cognito-user-pools-server-side-authentication-flow"></a>

If you don't have a user app, but instead you use a Java, Ruby, or Node\.js secure backend or server\-side app, you can use the authenticated server\-side API for Amazon Cognito user pools\.

For server\-side apps, user pool authentication is similar to authentication for client\-side apps, except for the following:
+ The server\-side app calls the `AdminInitiateAuth` API operation \(instead of `InitiateAuth`\)\. This operation requires AWS admin credentials\. The operation returns the required authentication parameters\.
+ After the server\-side app has the authentication parameters, it calls the `AdminRespondToAuthChallenge` API operation \(instead of `RespondToAuthChallenge`\)\. The `AdminRespondToAuthChallenge` API operation only succeeds when you provide AWS admin credentials\.

The `AdminInitiateAuth` and `AdminRespondToAuthChallenge` API operations can't accept username\-and\-password user credentials for admin sign\-in, unless you explicitly enable them to do so in one of the following ways:
+ Pass `ADMIN_USER_PASSWORD_AUTH` \(formerly known as `ADMIN_NO_SRP_AUTH`\) for the `ExplicitAuthFlow` parameter in your server\-side app's call to `CreateUserPoolClient` or `UpdateUserPoolClient`\.
+ Choose `Enable sign-in API for server-based authentication (ADMIN_USER_PASSWORD_AUTH)` on the **App integration** tab in your user pool\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.

## Custom authentication flow<a name="amazon-cognito-user-pools-custom-authentication-flow"></a>

Amazon Cognito user pools also make it possible to use custom authentication flows, which can help you create a challenge/response\-based authentication model using AWS Lambda triggers\.

**Note**  
You can't use advanced security features with custom authentication flows\. For more information, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

The custom authentication flow makes possible customized challenge and response cycles to meet different requirements\. The flow starts with a call to the `InitiateAuth` API operation that indicates the type of authentication to use and provides any initial authentication parameters\. Amazon Cognito responds to the `InitiateAuth` call with one of the following types of information: 
+ A challenge for the user, along with a session and parameters\.
+ An error if the user fails to authenticate\.
+ ID, access, and refresh tokens if the supplied parameters in the `InitiateAuth` call are sufficient to sign the user in\. \(Typically the user or app must first answer a challenge, but your custom code must determine this\.\)

 If Amazon Cognito responds to the `InitiateAuth` call with a challenge, the app gathers more input and calls the `RespondToAuthChallenge` operation\. This call provides the challenge responses and passes it back the session\. Amazon Cognito responds to the `RespondToAuthChallenge` call similarly to the `InitiateAuth` call\. If the user has signed in, Amazon Cognito provides tokens, or if the user isn't signed in, Amazon Cognito provides another challenge, or an error\. If Amazon Cognito returns another challenge, the sequence repeats and the app calls `RespondToAuthChallenge` until the user successfully signs in or an error is returned\. For more details about the `InitiateAuth` and `RespondToAuthChallenge` API operations, see the [API documentation](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html)\. 

## Built\-in authentication flow and challenges<a name="Built-in-authentication-flow-and-challenges"></a>

Amazon Cognito contains built\-in `AuthFlow` and `ChallengeName` values so that a standard authentication flow can validate a user name and password through the Secure Remote Password \(SRP\) protocol\. The AWS SDKs have built\-in support for these flows with Amazon Cognito\. 

The flow starts by sending `USER_SRP_AUTH` as the `AuthFlow` to `InitiateAuth`\. You also send `USERNAME` and `SRP_A` values in `AuthParameters`\. If the `InitiateAuth` call is successful, the response has included `PASSWORD_VERIFIER` as the `ChallengeName` and `SRP_B` in the challenge parameters\. The app then calls `RespondToAuthChallenge` with the `PASSWORD_VERIFIER` `ChallengeName` and the necessary parameters in `ChallengeResponses`\. If the call to `RespondToAuthChallenge` is successful and the user signs in, Amazon Cognito issues tokens\. If you activated multi\-factor authentication \(MFA\) for the user, Amazon Cognito returns the `ChallengeName` of `SMS_MFA`\. The app can provide the necessary code through another call to `RespondToAuthChallenge`\. 

## Custom authentication flow and challenges<a name="Custom-authentication-flow-and-challenges"></a>

An app can initiate a custom authentication flow by calling `InitiateAuth` with `CUSTOM_AUTH` as the `Authflow`\. With a custom authentication flow, three Lambda triggers control challenges and verification of the responses\.
+ The `DefineAuthChallenge` Lambda trigger uses a session array of previous challenges and responses as input\. It then generates the next challenge name and Booleans that indicate whether the user is authenticated and can be granted tokens\. This Lambda trigger is a state machine that controls the user’s path through the challenges\.
+ The `CreateAuthChallenge` Lambda trigger takes a challenge name as input and generates the challenge and parameters to evaluate the response\. When `DefineAuthChallenge` returns `CUSTOM_CHALLENGE` as the next challenge, the authentication flow calls `CreateAuthChallenge`\. The `CreateAuthChallenge` Lambda trigger passes the next type of challenge in the challenge metadata parameter\.
+ The `VerifyAuthChallengeResponse` Lambda function evaluates the response and returns a Boolean to indicate if the response was valid\.

A custom authentication flow can also use a combination of built\-in challenges, such as SRP password verification and MFA through SMS\. It can use custom challenges such as CAPTCHA or secret questions\.

## Use SRP password verification in custom authentication flow<a name="Using-SRP-password-verification-in-custom-authentication-flow"></a>

If you want to include SRP in a custom authentication flow, you must begin with SRP\.
+ To initiate SRP password verification in a custom flow, the app calls `InitiateAuth` with `CUSTOM_AUTH` as the `Authflow`\. In the `AuthParameters` map, the request from your app includes `SRP_A:` \(the SRP A value\) and `CHALLENGE_NAME: SRP_A`\.
+ The `CUSTOM_AUTH` flow invokes the `DefineAuthChallenge` Lambda trigger with an initial session of `challengeName: SRP_A` and `challengeResult: true`\. Your Lambda function responds with `challengeName: PASSWORD_VERIFIER`, `issueTokens: false`, and `failAuthentication: false`\.
+  The app next must call `RespondToAuthChallenge` with `challengeName: PASSWORD_VERIFIER` and the other parameters required for SRP in the `challengeResponses` map\. 
+ If Amazon Cognito verifies the password, `RespondToAuthChallenge` invokes the `DefineAuthChallenge` Lambda trigger with a second session of `challengeName: PASSWORD_VERIFIER` and `challengeResult: true`\. At that point, the `DefineAuthChallenge` Lambda trigger responds with `challengeName: CUSTOM_CHALLENGE` to start the custom challenge\.
+ If MFA is enabled for a user, after the password is verified, it is handled automatically\.

**Note**  
The Amazon Cognito hosted sign\-in webpage can't activate [Custom authentication challenge Lambda triggers](user-pool-lambda-challenge.md)\.

For more information about the Lambda triggers, including sample code, see [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.

## Admin authentication flow<a name="amazon-cognito-user-pools-admin-authentication-flow"></a>

Best practice for authentication is to use the API operations described in [Custom authentication flow](#amazon-cognito-user-pools-custom-authentication-flow) with SRP for password verification\. The AWS SDKs use that approach, and this approach helps them to use SRP\. However, if you want to avoid SRP calculations, an alternative set of admin API operations is available for secure backend servers\. For these backend admin implementations, use `AdminInitiateAuth` in place of `InitiateAuth`\. Also, use `AdminRespondToAuthChallenge` in place of `RespondToAuthChallenge`\. Because you can submit the password as plaintext, you do not have to do SRP calculations when you use these operations\. \. Here is an example:

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

These admin authentication operations require developer credentials and use the AWS Signature Version 4 \(SigV4\) signing process\. These operations are available in standard AWS SDKs, including Node\.js, which is convenient for Lambda functions\. To use these operations and have them accept passwords in plaintext, you must activate them for the app in the console\. Alternatively, you can pass `ADMIN_USER_PASSWORD_AUTH` for the `ExplicitAuthFlow` parameter in calls to `CreateUserPoolClient` or `UpdateUserPoolClient`\. The `InitiateAuth` and `RespondToAuthChallenge` operations do not accept the `ADMIN_USER_PASSWORD_AUTH` `AuthFlow`\.

In the `AdminInitiateAuth` response `ChallengeParameters`, the `USER_ID_FOR_SRP` attribute, if present, contains the user's actual user name, not an alias \(such as email address or phone number\)\. In your call to `AdminRespondToAuthChallenge`, in the `ChallengeResponses`, you must pass this user name in the `USERNAME` parameter\. 

**Note**  
Because backend admin implementations use the admin authentication flow, the flow doesn't support device tracking\. When you have turned on device tracking, admin authentication succeeds, but any call to refresh the access token fails\.

## User migration authentication flow<a name="amazon-cognito-user-pools-user-migration-authentication-flow"></a>

A user migration Lambda trigger helps migrate users from a legacy user management system into your user pool\. If you choose the `USER_PASSWORD_AUTH` authentication flow, users don't have to reset their passwords during user migration\. This flow sends your users' passwords to the service over an encrypted SSL connection during authentication\.

When you have migrated all your users, switch flows to the more secure SRP flow\. The SRP flow doesn't send any passwords over the network\.

To learn more about Lambda triggers, see [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.

For more information about migrating users with a Lambda trigger, see [Importing users into user pools with a user migration Lambda trigger](cognito-user-pools-import-using-lambda.md)\.