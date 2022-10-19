# Amazon Cognito information in CloudTrail<a name="amazon-cognito-info-in-cloudtrail"></a>

CloudTrail is turned on in your AWS account when you create the account\. When supported event activity occurs in Amazon Cognito, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing events with CloudTrail event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\.

For an ongoing record of events in your AWS account, including events for Amazon Cognito, create a trail\. A CloudTrail trail delivers log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Overview for creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail supported services and integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-list)
+ [Configuring amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail log files from multiple regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail log files from multiple accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

**Amazon Cognito User Pools**

Amazon Cognito supports logging for all of the actions listed on the [User pool actions](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_Operations.html) page as events in CloudTrail log files\. Amazon Cognito also logs the following request types to your hosted UI as events in CloudTrail\.


**Hosted UI operations in Cloudtrail**  

| Operation | Description | 
| --- | --- | 
| Login\_GET, CognitoAuthentication | A user views or submits credentials to your [Login endpoint](login-endpoint.md)\. | 
| OAuth2\_Authorize\_GET | A user views your [Authorize endpoint](authorization-endpoint.md)\. | 
| OAuth2Response\_GET, OAuth2Response\_POST | A user submits an IdP token to your /oauth2/idpresponse endpoint\. | 
| SAML2Response\_POST | A user submits an IdP SAML assertion to your /saml2/idpresponse endpoint\. | 
| Login\_OIDC\_SAML\_POST | A user enters a username at your [Login endpoint](login-endpoint.md) and matches with an [IdP identifier](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-integrating-3rd-party-saml-providers.html)\. | 
| Token\_POST | A user submits an authorization code to your [Token endpoint](token-endpoint.md)\. | 
| Signup\_GET, Signup\_POST | A user submits sign\-up information to your /signup endpoint\. | 
| Confirm\_GET, Confirm\_POST | A user submits a confirmation code in the hosted UI\. | 
| ResendCode\_POST | A user submits a request to resend a confirmation code in the hosted UI\. | 
| ForgotPassword\_GET, ForgotPassword\_POST | A user submits a request to reset their password to your /forgotPassword endpoint\. | 
| ConfirmForgotPassword\_GET, ConfirmForgotPassword\_POST | A user submits a code to your /confirmForgotPassword endpoint that confirms their ForgotPassword request\. | 
| ResetPassword\_GET, ResetPassword\_POST | A user submits a new password in the hosted UI\. | 
| Mfa\_GET, Mfa\_POST | A user submits a multi\-factor authentication \(MFA\) code in the hosted UI\. | 
| MfaOption\_GET, MfaOption\_POST | A user chooses their preferred method for MFA in the hosted UI\. | 
| Logout | A user signs out at your /logout endpoint\. | 
| SAML2Logout\_POST | A user signs out at your /saml2/logout endpoint\. | 
| Error\_GET | A user views an error page in the hosted UI\. | 
| UserInfo\_GET, UserInfo\_POST | A user or IdP exchanges information with your [UserInfo endpoint](userinfo-endpoint.md)\. | 
| Confirm\_With\_Link\_GET | A user submits a confirmation based on a link that Amazon Cognito sent in an email message\. | 
| Event\_Feedback\_GET | A user submits feedback to Amazon Cognito about an [advanced security features](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-advanced-security.html) event\. | 

**Note**  
Amazon Cognito records `UserSub` but not `UserName` in CloudTrail logs for requests that are specific to a user\. You can find a user for a given `UserSub` by calling the `ListUsers` API, and using a filter for sub\. 

**Amazon Cognito Federated Identities**
+ [CreateIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_CreateIdentityPool.html)
+ [DeleteIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_DeleteIdentityPool.html)
+ [DescribeIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_DescribeIdentityPool.html)
+ [GetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetIdentityPoolRoles.html)
+ [ListIdentityPools](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_ListIdentityPools.html)
+ [SetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_SetIdentityPoolRoles.html)
+ [UpdateIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_UpdateIdentityPool.html)

**Amazon Cognito Sync**

Amazon Cognito supports logging for all of the actions listed on the [Amazon Cognito Sync actions](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_Operations.html) page as events in CloudTrail log files\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.