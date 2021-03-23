# Checking for Compromised Credentials<a name="cognito-user-pool-settings-compromised-credentials"></a>

Amazon Cognito can detect if a user’s credentials \(user name and password\) have been compromised elsewhere\. This can happen when users reuse credentials at more than one site, or when they use passwords that are easy to guess\.

From the **Advanced security** page in the Amazon Cognito console, you can choose whether to allow, or block the user if compromised credentials are detected\. Blocking requires users to choose another password\. Choosing **Allow** publishes all attempted uses of compromised credentials to Amazon CloudWatch\. For more information, see [Viewing Advanced Security Metrics](user-pool-settings-viewing-advanced-security-metrics.md)\.

You can also choose whether Amazon Cognito checks for compromised credentials during sign\-in, sign\-up, and password changes\. 

![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[User event history\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Currently, Amazon Cognito doesn't check for compromised credentials for sign\-in operations with Secure Remote Password \(SRP\) flow, which doesn't send the password during sign\-in\. Sign\-ins that use the [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API with ADMIN\_NO\_SRP\_AUTH flow and the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API with USER\_PASSWORD\_AUTH flow are checked for compromised credentials\.

To add compromised credentials protections to your user pool, see [Adding Advanced Security to a User Pool](cognito-user-pool-settings-advanced-security.md)\.