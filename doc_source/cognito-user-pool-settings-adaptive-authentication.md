# Using adaptive authentication<a name="cognito-user-pool-settings-adaptive-authentication"></a>

With adaptive authentication, you can configure your user pool to block suspicious sign\-ins or add second factor authentication in response to an increased risk level\. For each sign\-in attempt, Amazon Cognito generates a risk score for how likely the sign\-in request is to be from a compromised source\. This risk score is based on many factors, including whether it detects a new device, user location, or IP address\. Adaptive Authentication adds MFA based on risk level for users who don't have an MFA type enabled at the user level\. When an MFA type is enabled at the user level, those users will always receive the second factor challenge during authentication regardless of how you configured adaptive authentication\.

Amazon Cognito publishes sign\-in attempts, their risk levels, and failed challenges to Amazon CloudWatch\. For more information, see [Viewing advanced security metrics](user-pool-settings-viewing-advanced-security-metrics.md)\.

To add adaptive authentication to your user pool, see [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)\.

**Topics**
+ [Adaptive authentication overview](#security-cognito-user-pool-settings-adaptive-authentication-overview)
+ [Creating a device fingerprint](#user-pool-settings-adaptive-authentication-device-fingerprint)
+ [Viewing user event history](#user-pool-settings-adaptive-authentication-event-user-history)
+ [Providing event feedback](#user-pool-settings-adaptive-authentication-feedback)
+ [Sending notification messages](#user-pool-settings-adaptive-authentication-messages)

## Adaptive authentication overview<a name="security-cognito-user-pool-settings-adaptive-authentication-overview"></a>

From the **Advanced security** page in the Amazon Cognito console, you can choose settings for adaptive authentication, including what actions to take at different risk levels and customization of notification messages to users\.




**For each risk level, you can choose from the following options:**  

| Option | Action | 
| --- | --- | 
| Allow | Users can sign in without an additional factor\. | 
| Optional MFA | Users who have a second factor configured must complete a second factor challenge to sign in\. A phone number for SMS and a TOTP software token are the available second factors\. Users without a second factor configured can sign in with only one set of credentials\. | 
| Require MFA | Users who have a second factor configured must complete a second factor challenge to sign in\. Amazon Cognito blocks sign\-in for users who don't have a second factor configured\. | 
| Block | Amazon Cognito blocks all sign\-in attempts at the designated risk level\. | 

**Note**  
You don't have to verify phone numbers to use them for SMS as a second authentication factor\.

## Creating a device fingerprint<a name="user-pool-settings-adaptive-authentication-device-fingerprint"></a>

When you call Amazon Cognito authentication API operations like [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) or [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) from your server, you must pass the source IP of the user in the ContextData parameter\. In addition, pass your server name, server path, and encoded\-device fingerprinting data\. Collect and submit this data with the Amazon Cognito context data collection library\. 

For information on enabling advanced security from your web or mobile app, see [Enabling user pool advanced security from your app](user-pool-settings-viewing-advanced-security-app.md)\.

When your app calls Amazon Cognito from your server, collect user context data from the client side\. The following is an example that uses the JavaScript SDK method `getData`\.

```
var encodedData = AmazonCognitoAdvancedSecurityData.getData(username, userPoolId, clientId);
```

When you are designing your app to use adaptive authentication, we recommend that you incorporate the latest Amazon Cognito SDK into your app\.\. The latest version of the SDK collects device fingerprinting information like device ID, model, and time zone\. For more information about Amazon Cognito SDKs, see [Install a user pool SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

## Viewing user event history<a name="user-pool-settings-adaptive-authentication-event-user-history"></a>

**Note**  
In the new Amazon Cognito console, you can view user event history in the **Users** tab\.

To see the sign\-in history for a user, you can choose the user from **Users and groups** in the Amazon Cognito console\. Amazon Cognito retains user event history for two years\.

![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Each sign\-in event has an event ID\. The event also has corresponding context data, such as location, device details, and risk detection results\. You can query user event history with the Amazon Cognito API operation [AdminListUserAuthEvents](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminListUserAuthEvents.html) or with the AWS Command Line Interface with [admin\-list\-user\-auth\-events](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-list-user-auth-events.html)\.

You can also correlate the event ID with the token that Amazon Cognito issued at the time that it recorded the event\. The ID and access tokens include this event ID in their payload\. Amazon Cognito also correlates refresh token use to the original event ID\. You can trace the original event ID back to the event ID of the sign\-in event that resulted in issuing the Amazon Cognito tokens\. You can trace token usage within your system to a particular authentication event\.

## Providing event feedback<a name="user-pool-settings-adaptive-authentication-feedback"></a>

Event feedback affects risk evaluation in real time and improves the risk evaluation algorithm over time\. You can provide feedback on the validity of sign\-in attempts through the Amazon Cognito console and API operations\.

The console lists the sign\-in history on the **Users and groups** tab\. If you select an entry, you can mark the event as valid or not valid\. You can also provide feedback through the user pool API operation [AdminUpdateAuthEventFeedback](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateAuthEventFeedback.html), and through the AWS CLI command [admin\-update\-auth\-event\-feedback](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-auth-event-feedback.html)\.

## Sending notification messages<a name="user-pool-settings-adaptive-authentication-messages"></a>

With advanced security protections, Amazon Cognito can notify your users of sign\-in attempts\. Amazon Cognito can also prompt users to select links to indicate if the sign\-in was valid or not valid\. Amazon Cognito uses this feedback to improve the risk detection accuracy for your user pool\. 

In the **How do you want to use adaptive authentication for sign\-in attempts rated as low, medium and high risk?** section choose **Notify Users** for the low, medium, and high\-risk cases\.

![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Notify users\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can customize notification emails, and provide both plaintext and HTML versions\. Choose **Customize** from **Adaptive authentication notification messages** to customize your email notifications\. To learn more about email templates, see [Message templates](cognito-user-pool-settings-message-templates.md)\.