# Using Adaptive Authentication<a name="cognito-user-pool-settings-adaptive-authentication"></a>

With adaptive authentication you can configure your user pool to block suspicious sign\-ins or add second factor authentication in response to an increased risk level\. For each sign\-in attempt, Amazon Cognito generates a risk score for how likely the sign\-in request is to be from a compromised source\. This risk score is based on many factors, including whether it detects a new device, user location, or IP address\. Adaptive Authentication adds MFA based on risk level for users who don't have an MFA type enabled at the user level\. When an MFA type is enabled at the user level, those users will always receive the second factor challenge during authentication regardless of how you configured adaptive authentication\.

Amazon Cognito publishes sign\-in attempts, their risk levels, and failed challenges to Amazon CloudWatch\. For more information, see [Viewing Advanced Security Metrics](user-pool-settings-viewing-advanced-security-metrics.md)\.

To add adaptive authentication to your user pool, see [Adding Advanced Security to a User Pool](cognito-user-pool-settings-advanced-security.md)\.

**Topics**
+ [Adaptive Authentication Overview](#security-cognito-user-pool-settings-adaptive-authentication-overview)
+ [Creating a Device Fingerprint](#user-pool-settings-adaptive-authentication-device-fingerprint)
+ [Viewing User Event History](#user-pool-settings-adaptive-authentication-event-user-history)
+ [Providing Event Feedback](#user-pool-settings-adaptive-authentication-feedback)
+ [Sending Notification Messages](#user-pool-settings-adaptive-authentication-messages)

## Adaptive Authentication Overview<a name="security-cognito-user-pool-settings-adaptive-authentication-overview"></a>

From the **Advanced security** page in the Amazon Cognito console, you can choose settings for adaptive authentication, including what actions to take at different risk levels and customization of notification messages to users\.




**For each risk level, you can choose from the following options:**  

| Option | Action | 
| --- | --- | 
| Allow | The sign\-in attempt is allowed without an additional factor\. | 
| Optional MFA | Users who have a second factor such as phone number\* for SMS or a TOTP software token configured are required to complete a second factor challenge to sign in\. Users who don't have a second factor configured are allowed to sign in without an additional factor\. | 
| Require MFA | Users who have a second factor such as phone number\* for SMS or a TOTP software token configured are required to complete a second factor challenge to sign in\. Users who don't have a second factor configured are blocked from signing in\. | 
| Block | All sign\-in attempts at that risk level are blocked\. | 

**Note**  
\*Phone numbers don't need to be verified to be used for SMS as a second authentication factor\.

## Creating a Device Fingerprint<a name="user-pool-settings-adaptive-authentication-device-fingerprint"></a>

When you call the Amazon Cognito authentication APIs, such as [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) or [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) from your server, you need to pass the source IP of the user in the ContextData parameter\. This is in addition to your server name, server path, and encoded\-device fingerprinting data, which are collected by using the Amazon Cognito context data collection library\. 

For information on enabling advanced security from your web or mobile app, see [Enabling User Pool Advanced Security from Your App](user-pool-settings-viewing-advanced-security-app.md)\.

Collect user context data from the client side when your app is calling Amazon Cognito from your server\. The following is an example that uses the JavaScript SDK method `getData`\.

```
var encodedData = AmazonCognitoAdvancedSecurityData.getData(username, userPoolId, clientId);
```

We recommend incorporating the latest Amazon Cognito SDK into your app\. This enables adaptive authentication to collect device fingerprinting information—such as device ID, model, and time zone\. For more information about Amazon Cognito SDKs, see [Install a user pool SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

## Viewing User Event History<a name="user-pool-settings-adaptive-authentication-event-user-history"></a>

You can choose a user in the Amazon Cognito console from **Users and groups** to see the sign\-in history for that user\. User event history is retained for two years by Amazon Cognito\.

![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Each sign\-in event has an event ID, context data such as location, device details, and risk detection results associated with it\.

![\[User sign-in event\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User sign-in event\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User sign-in event\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can correlate the event id to the token issued as well\. The issued token, such as the ID token and access token, carries this event ID in its payload\. Even using the refresh token persists the original event ID\. The original event ID can be traced back to the event ID of the sign\-in event that resulted in issuing the Amazon Cognito tokens\. This enables you to trace usage of a token within your system to a particular authentication event\.

## Providing Event Feedback<a name="user-pool-settings-adaptive-authentication-feedback"></a>

Event feedback affects risk evaluation in real time, and improves the risk evaluation algorithm over time\. You can provide feedback on the validity of sign\-in attempts through the Amazon Cognito console and API operations\.

In the console, on the **Users and groups** tab, the sign\-in history is listed, and if you click an entry, you can mark the event as valid or invalid\. You can also provide feedback through the user pool API method [AdminUpdateAuthEventFeedback](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateAuthEventFeedback.html), and through the AWS CLI command [admin\-update\-auth\-event\-feedback](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-auth-event-feedback.html)\.

## Sending Notification Messages<a name="user-pool-settings-adaptive-authentication-messages"></a>

With advanced security protections, Amazon Cognito can notify your users of sign\-in attempts, prompt them to click links to indicate if the sign\-in was valid or invalid, and use their feedback to improve the risk detection accuracy for your user pool\. 

In the **How do you want to use adaptive authentication for sign\-in attempts rated as low, medium and high risk?** section choose **Notify Users** for the low, medium, and high\-risk cases\.

![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can customize notification emails, and provide both plaintext and HTML versions\. Choose **Customize** from **Adaptive authentication notification messages** to customize your email notifications\. To learn more about email templates, see [Message Templates](cognito-user-pool-settings-message-templates.md)\.