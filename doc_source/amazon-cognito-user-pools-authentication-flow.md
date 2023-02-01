# User pool authentication flow<a name="amazon-cognito-user-pools-authentication-flow"></a>

Amazon Cognito includes several methods to authenticate your users\. All user pools, whether you have a domain or not, can authenticate users in the native API\. If you add a domain to your user pool, you can use the [OIDC API endpoints](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html)\. The native API supports a variety of authorization models and request flows for API requests\.

To verify the identity of users, Amazon Cognito supports authentication flows that incorporate new challenge types, in addition to passwords\. Amazon Cognito authentication typically requires that you implement two API operations in the following order: 

1. `InitiateAuth`

1. `RespondToAuthChallenge`

A user authenticates by answering successive challenges until authentication either fails or Amazon Cognito issues tokens to the user\. You can repeat these steps with Amazon Cognito, in a process that includes different challenges, to support any custom authentication flow\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Typically, your app generates a prompt to gather information from your user, and submits that information in an API request to Amazon Cognito\. Consider an `InitiateAuth` flow in a user pool where you have configured your user with multi\-factor authentication \(MFA\)\. 

1. Your app prompts your user for their user name and password\.

1. You include the user name and password as parameters in `InitiateAuth`\.

1. Amazon Cognito returns an `SMS_MFA` challenge and a session identifier\.

1. Your app prompts your user for the MFA code from their phone\.

1. You include that code and the session identifier in the `RespondToAuthChallenge` request\.

Depending on the features of your user pool, you can end up responding to several challenges to `InitiateAuth` before your app retrieves tokens from Amazon Cognito\. Amazon Cognito includes a session string in the response to each request\. To combine your API requests into an authentication flow, include the session string from the response to the previous request in each subsequent request\. By default, your users have three minutes to complete each challenge before the session string expires\. To adjust this period, change your app client **Authentication flow session duration**\. The following procedure describes how to change this setting in your app client configuration\.

------
#### [ Amazon Cognito console ]

**To configure app client authentication flow session duration \(AWS Management Console\)**

1. From the **App integration** tab in your user pool, select the name of your app client from the **App clients and analytics** container\.

1. Select **Edit** in the **App client information** container\.

1. Change the value of **Authentication flow session duration** to the validity duration that you want, in minutes, for SMS MFA codes\. This also changes the amount of time that any user has to complete any authentication challenge in your app client\.

1. Select **Save changes**\.

------
#### [ Amazon Cognito API ]

**To configure app client authentication flow session duration \(Amazon Cognito API\)**

1. Prepare an `UpdateUserPoolClient` request with your existing user pool settings from a `DescribeUserPoolClient` request\. Your `UpdateUserPoolClient` request must include all existing app client properties\.

1. Change the value of `AuthSessionValidity` to the validity duration that you want, in minutes, for SMS MFA codes\. This also changes the amount of time that any user has to complete any authentication challenge in your app client\.

------

For more information about app clients, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.

You can use AWS Lambda triggers to customize the way users authenticate\. These triggers issue and verify their own challenges as part of the authentication flow\.

You can also use the admin authentication flow for secure backend servers\. You can use the user migration authentication flow to make user migration possible without the requirement that your users to reset their passwords\.

**Amazon Cognito lockout behavior for failed sign\-in attempts**  
After five failed sign\-in attempts, Amazon Cognito locks out your user for one second\. The lockout duration then doubles after each additional one failed attempt, up to a maximum of approximately 15 minutes\. Attempts made during a lockout period generate a `Password attempts exceeded` exception, and don't affect the duration of subsequent lockout periods\. For a cumulative number of failed sign\-in attempts *n*, not including `Password attempts exceeded` exceptions, Amazon Cognito locks out your user for *2^\(n\-5\)* seconds\. To reset the lockout to its initial state, your user must not initiate any sign\-in attempts for 15 consecutive minutes at any time after a lockout\. This behavior is subject to change\.

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

The following process works for user client\-side apps that you create with [AWS Amplify](https://docs.amplify.aws/lib/auth/getting-started/) or the [AWS SDKs](http://aws.amazon.com/developer/tools/)\.

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
+ The server\-side app calls the `AdminInitiateAuth` API operation \(instead of `InitiateAuth`\)\. This operation requires AWS credentials with permissions that include `cognito-idp:AdminInitiateAuth` and `cognito-idp:AdminRespondToAuthChallenge`\. The operation returns the required authentication parameters\.
+ After the server\-side app has the authentication parameters, it calls the `AdminRespondToAuthChallenge` API operation \(instead of `RespondToAuthChallenge`\)\. The `AdminRespondToAuthChallenge` API operation only succeeds when you provide AWS credentials\.

For more information about signing Amazon Cognito API requests with AWS credentials, see [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.

The `AdminInitiateAuth` and `AdminRespondToAuthChallenge` API operations can't accept username\-and\-password user credentials for admin sign\-in, unless you explicitly enable them to do so in one of the following ways:
+ Include `ALLOW_ADMIN_USER_PASSWORD_AUTH` \(formerly known as `ADMIN_NO_SRP_AUTH`\) in the `ExplicitAuthFlow` parameter when you call `CreateUserPoolClient` or `UpdateUserPoolClient`\.
+ Add `ALLOW_ADMIN_USER_PASSWORD_AUTH` to the list of **Authentication flows** for you app client\. Configure app clients on the **App integration** tab in your user pool, under **App clients and analytics**\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.

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
+ If MFA is enabled for a user, after Amazon Cognito verifies the password, your user is then challenged to set up or sign in with MFA\.

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