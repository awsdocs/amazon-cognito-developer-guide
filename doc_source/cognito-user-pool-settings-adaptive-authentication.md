# Using adaptive authentication<a name="cognito-user-pool-settings-adaptive-authentication"></a>

With adaptive authentication, you can configure your user pool to block suspicious sign\-ins or add second factor authentication in response to an increased risk level\. For each sign\-in attempt, Amazon Cognito generates a risk score for how likely the sign\-in request is to be from a compromised source\. This risk score is based on factors that include device and user information\. Adaptive authentication can turn on or require multi\-factor authentication \(MFA\) for a user in your user pool when Amazon Cognito detects risk in a user's session, and the user hasn't yet chosen an MFA method\. When you activate MFA for a user, they always receive a challenge to provide or set up a second factor during authentication, regardless of how you configured adaptive authentication\. From your user's perspective, your app offers to help them set up MFA, and optionally Amazon Cognito prevents them from signing in again until they have configured an additional factor\.

Amazon Cognito publishes sign\-in attempts, their risk levels, and failed challenges to Amazon CloudWatch\. For more information, see [Viewing advanced security metrics](user-pool-settings-viewing-advanced-security-metrics.md)\.

To add adaptive authentication to your user pool, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

**Topics**
+ [Adaptive authentication overview](#security-cognito-user-pool-settings-adaptive-authentication-overview)
+ [Adding user device and session data to API requests](#user-pool-settings-adaptive-authentication-device-fingerprint)
+ [Viewing user event history](#user-pool-settings-adaptive-authentication-event-user-history)
+ [Providing event feedback](#user-pool-settings-adaptive-authentication-feedback)
+ [Sending notification messages](#user-pool-settings-adaptive-authentication-messages)

## Adaptive authentication overview<a name="security-cognito-user-pool-settings-adaptive-authentication-overview"></a>

From **Advanced security** in the **App integration** tab of the Amazon Cognito console, you can choose settings for adaptive authentication, including what actions to take at different risk tlevels and customization of notification messages to users\. You can assign a global advanced security configuration to all of your app clients, but apply a client\-level configuration to individual app clients\.

Consider your options carefully when you change your **Enforcement method** from **Audit\-only** to **Full\-function**\. The automatic responses that you apply to risk levels influence the risk level that Amazon Cognito assigns to subsequent user sessions with the same characteristics\. For example, after you choose to take no action, or **Allow**, user sessions that Amazon Cognito initially evaluates to be high\-risk, Amazon Cognito considers similar sessions to have a lower risk\.


**For each risk level, you can choose from the following options:**  

| Option | Action | 
| --- | --- | 
| Allow | Users can sign in without an additional factor\. | 
| Optional MFA | Users who have a second factor configured must complete a second factor challenge to sign in\. A phone number for SMS and a TOTP software token are the available second factors\. Users without a second factor configured can sign in with only one set of credentials\. | 
| Require MFA | Users who have a second factor configured must complete a second factor challenge to sign in\. Amazon Cognito blocks sign\-in for users who don't have a second factor configured\. | 
| Block | Amazon Cognito blocks all sign\-in attempts at the designated risk level\. | 

**Note**  
You don't have to verify phone numbers to use them for SMS as a second authentication factor\.

## Adding user device and session data to API requests<a name="user-pool-settings-adaptive-authentication-device-fingerprint"></a>

You can collect and pass information about your user's session to Amazon Cognito advanced security when you use the API to sign them up, sign them in, and reset their password\. This information includes your user's IP address and a unique device identifier\.

You might have a intermediate network device between your users and Amazon Cognito, like a proxy service or an application server\. You can collect users' context data and pass it to Amazon Cognito so that adaptive authentication calculates your risk based on the characteristics of the user endpoint, instead of your server or proxy\. If your client\-side app calls Amazon Cognito API operations directly, adaptive authentication automatically records the source IP address\. However, it does not record other device information like the `user-agent` unless you also collect a device fingerprint\.

Generate this data with the Amazon Cognito context data collection library and submit it to Amazon Cognito advanced security with the [ContextData](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ContextDataType.html) and [UserContextData](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UserContextDataType.html) parameters\. The context data collection library is included in the AWS SDKs\. For more information, see [Integrating Amazon Cognito with web and mobile apps](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-integrate-apps.html)\. You can submit `ContextData` if you have activated advanced security features in your user pool\. For more information, see [Configuring advanced security features](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-advanced-security.html#cognito-user-pool-configure-advanced-security)\.

When you call the following Amazon Cognito authenticated API operations from your app server, pass the IP of the user’s device in the `ContextData` parameter\. In addition, pass your server name, server path, and encoded device\-fingerprinting data\.
+ [AdminInitiateAuth ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html)
+ [AdminRespondToAuthChallenge ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html)

When you call Amazon Cognito unauthenticated API operations, you can submit `UserContextData` to Amazon Cognito advanced security features\. This data includes a device fingerprint in the `EncodedData` parameter\. You can also submit an `IpAddress` parameter in your `UserContextData` if you meet the following conditions:
+ You have activated advanced security features in your user pool\. For more information, see [ Configuring advanced security features](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-advanced-security.html#cognito-user-pool-configure-advanced-security)\.
+ Your app client has a client secret\. For more information, see [Configuring a user pool app client](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-idp-settings.html)\.
+ You have activated **Accept additional user context data** in your app client\. For more information, see [ Accepting additional user context data \(AWS console\)](#user-pool-settings-adaptive-authentication-accept-user-context-data)\.

Your app can populate the `UserContextData` parameter with encoded device\-fingerprinting data and the IP address of the user's device in the following Amazon Cognito unauthenticated API operations\.
+ [InitiateAuth ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html)
+ [RespondToAuthChallenge ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html)
+ [SignUp ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)
+ [ConfirmSignUp ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html)
+ [ForgotPassword ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html)
+ [ConfirmForgotPassword ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmForgotPassword.html)
+ [ResendConfirmationCode ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ResendConfirmationCode.html)

### Accepting additional user context data \(AWS console\)<a name="user-pool-settings-adaptive-authentication-accept-user-context-data"></a>

Your user pool accepts an IP address in a `UserContextData` parameter after you activate the **Accept additional user context data** feature\. You don’t need to activate this feature if:
+ Your users only sign in with authenticated API operations like [AdminInitiateAuth ](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html), and you use the `ContextData` parameter\.
+ You only want your unauthenticated API operations to send a device fingerprint, but not an IP address, to Amazon Cognito advanced security features\.

Update your app client as follows in the Amazon Cognito console to add support for additional user context data\.

1. Sign in to the [Amazon Cognito console ](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, choose **Manage your User Pools**, and choose the user pool you want to edit\.

1. Choose the **App integration** tab\.

1. Under **App clients and analytics**, choose or create an app client\. For more information, see [ Configuring a user pool app client](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-idp-settings.html)\.

1. Choose **Edit** from the **App client information** container\.

1. In the **Advanced authentication settings** for your app client, choose **Accept additional user context data**\.

1. Choose **Save changes**\.

To configure your app client to accept user context data in the Amazon Cognito API, set `EnablePropagateAdditionalUserContextData` to `true` in a [CreateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html) or [UpdateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html) request\. For information about how to activate advanced security from your web or mobile app, see [Activating user pool advanced security from your app](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-viewing-advanced-security-app.html)\. When your app calls Amazon Cognito from your server, collect user context data from the client side\. The following is an example that uses the JavaScript SDK method `getData`\.

```
var encodedData = AmazonCognitoAdvancedSecurityData.getData(username, userPoolId, clientId);
```

When you design your app to use adaptive authentication, we recommend that you incorporate the latest Amazon Cognito SDK into your app\.\. The latest version of the SDK collects device fingerprinting information like device ID, model, and time zone\. For more information about Amazon Cognito SDKs, see [Install a user pool SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\. Amazon Cognito advanced security only saves and assigns a risk score to events that your app submits in the correct format\. If Amazon Cognito returns an error response, check that your request includes a valid secret hash and that the `IPaddress` parameter is a valid IPv4 or IPv6 address\.

## Viewing user event history<a name="user-pool-settings-adaptive-authentication-event-user-history"></a>

**Note**  
In the new Amazon Cognito console, you can view user event history in the **Users** tab\.

To see the sign\-in history for a user, you can choose the user from the **Users** tab in the Amazon Cognito console\. Amazon Cognito retains user event history for two years\.

![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Each sign\-in event has an event ID\. The event also has corresponding context data, such as location, device details, and risk detection results\. You can query user event history with the Amazon Cognito API operation [AdminListUserAuthEvents](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminListUserAuthEvents.html) or with the AWS Command Line Interface \(AWS CLI\) with [admin\-list\-user\-auth\-events](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-list-user-auth-events.html)\.

You can also correlate the event ID with the token that Amazon Cognito issued at the time that it recorded the event\. The ID and access tokens include this event ID in their payload\. Amazon Cognito also correlates refresh token use to the original event ID\. You can trace the original event ID back to the event ID of the sign\-in event that resulted in issuing the Amazon Cognito tokens\. You can trace token usage within your system to a particular authentication event\. For more information, see [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

## Providing event feedback<a name="user-pool-settings-adaptive-authentication-feedback"></a>

Event feedback affects risk evaluation in real time and improves the risk evaluation algorithm over time\. You can provide feedback on the validity of sign\-in attempts through the Amazon Cognito console and API operations\. 

**Note**  
Your event feedback influences the risk level that Amazon Cognito assigns to subsequent user sessions with the same characteristics\.

In the Amazon Cognito console, choose a user from the **Users** tab and select **Provide event feedback**\. You can review the event details and **Set as valid** or **Set as invalid**\.

The console lists the sign\-in history on the **Users and groups** tab\. If you select an entry, you can mark the event as valid or not valid\. You can also provide feedback through the user pool API operation [AdminUpdateAuthEventFeedback](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateAuthEventFeedback.html), and through the AWS CLI command [admin\-update\-auth\-event\-feedback](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-auth-event-feedback.html)\. 

When you select **Set as valid** in the Amazon Cognito console or provide a `FeedbackValue` value of `valid` in the API, you tell Amazon Cognito that you trust a user session where Amazon Cognito has evaluated some level of risk\. When you select **Set as invalid** in the Amazon Cognito console or provide a `FeedbackValue` value of `invalid` in the API, you tell Amazon Cognito that you don't trust a user session, or you don't believe that Amazon Cognito evaluated a high\-enough risk level\.

## Sending notification messages<a name="user-pool-settings-adaptive-authentication-messages"></a>

With advanced security protections, Amazon Cognito can notify your users of sign\-in attempts\. Amazon Cognito can also prompt users to select links to indicate if the sign\-in was valid or not valid\. Amazon Cognito uses this feedback to improve the risk detection accuracy for your user pool\. 

In the **Automatic risk response** section choose **Notify Users** for the low, medium, and high\-risk cases\.

![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can customize notification email messages, and provide both plaintext and HTML versions of these messages\. Choose **Customize** from **Adaptive authentication notification messages** to customize your email notifications\. To learn more about email templates, see [Message templates](cognito-user-pool-settings-message-templates.md)\.