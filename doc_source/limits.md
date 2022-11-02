# Quotas in Amazon Cognito<a name="limits"></a>

Amazon Cognito has default quotas, formerly referred to as *limits*, for the maximum number of operations that you can perform in your account\. Amazon Cognito also has quotas for the maximum number and size of Amazon Cognito resources\.

**[Operation quotas](#operation-quotas)**
+ [Quota categorization](#quota-categorization)
+ [Amazon Cognito user pools API operations with special request rate handling ](#api-operation-special-handling)
+ [Amazon Cognito user pools API operation categories and request rate quotas](#category_operations)
+ [Track quota usage](#track-quota-usage)
+ [Optimize quotas](#optimize-quotas)
+ [Identify quota requirements](#identify-quota-requirements)
+ [Requesting a quota increase](#api-request-rate-quotas)

**[Resource quotas](#resource-quotas)**

## Operation quotas<a name="operation-quotas"></a>

### Quota categorization<a name="quota-categorization"></a>

Amazon Cognito has quotas for the maximum number of operations, such as `InitiateAuth` or `RespondToAuthChallenge`, that you can use to perform certain user actions in your applications\. These operations are grouped into categories of common use cases, such as `UserAuthentication` or `UserCreation`\. For a list of categorized operations, see [Amazon Cognito user pools API operation categories and request rate quotas](#category_operations)\. You can track your quota usage, and request increases, by category in the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home)\.

Operation quotas are defined as the maximum number of requests per second \(RPS\) for all operations within a category\. The Amazon Cognito user pools service applies quotas to all operations in each category\. For example, the category `UserCreation` includes four operations: `SignUp`, `ConfirmSignUp`, `AdminCreateUser`, and `AdminConfirmSignUp`\. It's allocated with a combined quota of 50 RPS\. If multiple operations take place at the same time, each operation within this category can call up to 50 RPS separately or combined\. 

**Note**  
The category quota is enforced for each AWS account across all user pools in an account and Region\.

### Amazon Cognito user pools API operations with special request rate handling<a name="api-operation-special-handling"></a>

Operation quotas are measured and enforced for the combined total requests at the category level, except for the `AdminRespondToAuthChallenge` and `RespondToAuthChallenge` operations, where special handling rules are applied\. 

The `UserAuthentication` category includes four operations: `AdminInitiateAuth`, `InitiateAuth`, `AdminRespondToAuthChallenge`, and `RespondToAuthChallenge`\. The `InitiateAuth` and `AdminInitiateAuth` operations are measured and enforced per category quota\. The matching operations `RespondToAuthChallenge` and `AdminRespondToAuthChallenge` are subject to a separate quota that is three times the `UserAuthentication` category limit\. This elevated quota accommodates multiple authentication challenges set up in developers’ apps\. The quota is sufficient to cover the large majority of use cases\. Beyond the three\-times\-per\-authentication\-call threshold, the excess rate counts towards the `UserAuthentication` category quota\.

For example, if your quota for the `UserAuthentication` category is 80 RPS, you can call `RespondToAuthChallenge` or `AdminRespondToAuthChallenge` up to 240 RPS \(80 RPS x 3\)\. If the app is set up to have four rounds of challenge per authentication call and you have 70 sign\-ins per second, then the total `RespondToAuthChallenge` is 280 RPS \(70 x 4\), which is 40 RPS above the quota\. The extra 40 RPS is added to 70 `InitiateAuth` calls, making the total usage of `UserAuthentication` category 110 RPS \(40 \+ 70\)\. This value exceeds the category quota set at 80 RPS by 30 RPS, so this app is throttled\.

### Monthly active users<a name="monthly-active-users"></a>

Monthly active users \(MAUs\) are the benchmark by which Amazon Cognito user pools billing is calculated\. MAUs are also used to determine if your user pool is eligible for a quota increase\. A user is counted as a MAU if, within a calendar month, there is an identity operation related to that user, such as sign\-up, sign\-in, token refresh, password change, or a user account attribute is updated\. Amazon Cognito also counts a user as active in the month that you take an action that deactivates or deletes the user or its attributes\.

### Amazon Cognito user pools API operation categories and request rate quotas<a name="category_operations"></a>

Mappings between operations and their respective categories appear in the following table\. You can only request to increase adjustable category quotas\. For more information, see [Requesting a quota increase](#api-request-rate-quotas)\. Adjustable quotas are applied at the account level\. Some category operations1 are constrained at the user pool level to 5 RPS, while the **Default quota** applies to all user pools in an account\.

**Note**  
The quota for each category is measured in Monthly Active Users \(MAUs\)\. The default quota is intended for accounts with fewer than two million MAUs\. If you have less than one million MAUs, you should not request a quota increase\. 

Category operation quotas are applied across all users in a user pool\. Per\-user quotas also apply\. Your app can request up to 10 *read* operations on each user every second, including signing in or getting profile or device information\. Your app can also request up to 10 *write* operations on each user every second, including updating profile information or multi\-factor authentication \(MFA\) settings\. You can't change per\-user quotas\.


| Category | Description | Default quota \(RPS\)2 | Adjustable | Quota increase eligibility3 | 
| --- | --- | --- | --- | --- | 
| UserAuthentication[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that authenticate \(sign in\) a user\. These operations are subject to [Amazon Cognito user pools API operations with special request rate handling ](#api-operation-special-handling)\. | 120 | Yes | 40 RPS for each additional 1 million MAUs | 
| UserCreation[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that create or confirm an Amazon Cognito native user\. This is a user that is created and verified directly by your Amazon Cognito user pools\. | 50 | Yes | 10 RPS for each additional 1 million MAUs  | 
| UserFederationAmazon Cognito managed operations to federate a user through a third\-party IdP into Amazon Cognito\. | Operations that federate \(authenticate\) users with a third\-party identity provider into your Amazon Cognito user pools\. | 25 | Yes | 10 RPS for each additional 1 million MAUs | 
| UserAccountRecovery[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that recover a user's account, or change or update a user's password\. | 30 | No | N/A | 
| UserRead[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that retrieve a user from your user pools\.  | 120 | Yes | 40 RPS for each additional 1 million MAUs  | 
| UserUpdate[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) |  Operations that you use to manage users and user attributes\. | 25 | No | N/A | 
| UserToken[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations for token management | 120 | Yes | 40 RPS for each additional 1 million MAUs | 
| UserResourceRead[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that retrieve user resource information from Amazon Cognito, such as a remembered device or a group membership\. | 50 | Yes | 20 RPS for each additional 1 million MAUs  | 
| UserResourceUpdate[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that update resource information for a user, such as a remembered device or a group membership\. | 25 | No | N/A | 
| UserList[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that return a list of users\. | 30 | No | N/A | 
| UserPoolRead[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that read your user pools\. | 15 | No | N/A | 
| UserPoolUpdate[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that create, update, or delete your user pools\. | 15 | No | N/A | 
| UserPoolResourceRead[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that retrieve information about resources, such as groups or resource servers, from a user pool\.1 | 20 | No | N/A | 
| UserPoolResourceUpdate[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that modify resources, such as groups or resource servers, in a user pool\.1 | 15 | No | N/A | 
| UserPoolClientRead[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that retrieve information about your user pool clients\.1 | 15 | No | N/A | 
| UserPoolClientUpdate[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) | Operations that create, update, and delete your user pool clients\.1 | 15 | No | N/A | 
| ClientAuthentication`client_credentials` grant type requests to the TOKEN endpoint\. | Operations that generate credentials to be used in authorizing machine\-to\-machine requests | 150 | No | N/A | 

1 Any individual operation in this category has a constraint that prevents the operation from being called at a rate higher than 5 RPS for a single user pool\.

2 Request rate for all user pools with fewer than 2 million MAUs\.

3 Request rate increases will be considered when you have between 1 million and 10 million MAUs\. Quota increase requests are evaluated based on current utilization, existing or expected MAUs, and your implementation of optimization best practices\.Amazon Cognito reserves the right to deny quota increase requests to prevent misuse and protect service availability for all customers\. If you have more than 10 million MAUs, we recommend that you work with an AWS solutions architect to design your solution in a way that is optimized for Amazon Cognito quotas\. Please contact your AWS account team for help\. 

### Track quota usage<a name="track-quota-usage"></a>

Amazon Cognito generates `CallCount` and `ThrottleCount` metrics in Amazon CloudWatch for each API operation category at the account level\. You can use `CallCount` to track the total number of calls customers made related to a category\. You can use `ThrottleCount` to track the total number of throttled calls related to a category\. You can use the `CallCount` and `ThottleCount` metrics with the `Sum` statistic to count the total number of calls in a category\. For more information, see [CloudWatch usage metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)\.

When monitoring service quotas, *utilization* is the percentage of a service quota in use\. For example, if the quota value is 200 resources, and 150 resources are in use, the utilization is 75%\. *Usage* is the number of resources or operations in use for a service quota\.

**Tracking usage through CloudWatch metrics**

You can track and collect Amazon Cognito user pools utilization metrics using CloudWatch\. The CloudWatch dashboard displays metrics about every AWS service that you use\. With CloudWatch, you can create metric alarms to notify you or change a specific resource that you are monitoring\. For more information about CloudWatch metrics, see [Track your CloudWatch usage metrics](tracking-quotas-and-usage-in-cloud-watch-and-service-quotas.md)\.

**Tracking utilization through Service Quotas metrics**

Amazon Cognito user pools are integrated with Service Quotas, which is a browser\-based interface that you can use to view and manage your service quota usage\. In the Service Quotas console, you can look up the value of a specific quota, view monitoring information, request a quota increase, or set up CloudWatch alarms\. After your account has been active a while, you can view a graph of your resource utilization\.

For more information on viewing quotas in the Service Quotas console, see [Viewing Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/gs-request-quota.html)\. 

### Identify quota requirements<a name="identify-quota-requirements"></a>

**Important**  
If you increase Amazon Cognito quotas for categories such as `UserAuthentication`, `UserCreation`, or `AccountRecovery`, you may need to increase quotas for other services\. For example, messages that Amazon Cognito sends with Amazon Simple Notification Service \(Amazon SNS\) or Amazon Simple Email Service \(Amazon SES\) can fail if request rate quotas are insufficient in those services\.

To calculate quota requirements, determine how many active users will interact with your application in a specific time period\. For example, if you expect your application to sign in an average of one million active users within an eight\-hour period, then you must be able to authenticate an average of 35 users per second\. 

In addition, if you assume that the average user session is two hours, and you configure tokens to expire after an hour, each user must refresh their tokens once during their session\. The required average quota for the `UserAuthentication` category to support this load is 70 RPS\.

If you assume a peak\-to\-average ratio of 3:1 by accounting for the variance of user sign\-in frequency during the eight\-hour period, then you need the desired `UserAuthentication` quota of 200 RPS\. 

**Note**  
If you call multiple operations for each user action, you must sum up the individual operation call rates at the category level\.

### Optimize quotas<a name="optimize-quotas"></a>

Follow one of the following methods to handle a peak call rate\.

**Retry the attempt after a back\-off waiting period**  
You can catch errors with each API call, and then re\-try the attempt after a back\-off period\. You can adjust the back\-off algorithm according to business needs and load\. Amazon SDKs have built\-in retry logic\. For more information, see [ Tools to Build on AWS\.](https://aws.amazon.com/tools/ ) 

**Use an external database for frequently updated attributes**  
If your application requires several calls to a user pool to read or write custom attributes, use external storage\. You can use your preferred database to store custom attributes or use a cache layer to load a user profile during sign\-in\. You can reference this profile from the cache when needed, instead of reloading the user profile from a user pool\.

**Validate JWT tokens on the client side**  
Applications must validate JWT tokens before trusting them\. You can verify the signature and validity of tokens on the client side without sending API calls to a user pool\. After the token is validated, you can trust claims in the token and use the claims instead of making more `getUser` API calls\. For more information, see [ Verifying a JSON Web Token\.](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-verifying-a-jwt.html)

**Throttle traffic to your web application with a waiting room**  
If you expect traffic from a large number of users signing in during a time\-bound event, such as taking an exam or attending a live event, you can optimize request traffic with self\-throttling mechanisms\. You can, for example, set up a waiting room where users can stand by until a session is available, allowing you to process requests when you have available capacity\. See the [AWS Virtual Waiting Room solution](https://aws.amazon.com/solutions/implementations/aws-virtual-waiting-room) for a reference architecture of a waiting room\. 

### Requesting a quota increase<a name="api-request-rate-quotas"></a>

Amazon Cognito has a quota for the maximum number of user pool operations that you can perform in your account\. You can request an increase to the adjustable API request rate quotas in Amazon Cognito\. To request a quota increase, use the Service Quotas console, the Service limit increase form, or the `RequestServiceQuotaIncrease` or `ListAWSDefaultServiceQuotas` API operations\.
+ To request a quota increase using the Service Quotas console, see [Requesting a API quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.
+ If the quota isn't available in Service Quotas, use the [Service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\.

**Important**  
Only adjustable quotas can be increased\. Quota increase requests are evaluated based on current utilization, existing or expected MAUs, and your implementation of optimization best practices\. When you submit a quota increase request, provide as much information about your current usage, projected usage, and optimization methods as you are able to\. For quota increases that increase your request rate, your current average request rate should already be close to the maximum rate at your current quota\. See [Amazon Cognito user pools API operation categories and request rate quotas](#category_operations) to learn more about adjustable quotas\.   
Consider an example scenario where your application is growing to 5 million MAUs\. Your current average utilization in the UserAuthentication category operations is high: 60%, or 72 requests per second against your current quota of 120\. You can request a UserAuthentication quota increase for 40 requests per second for each of the 3 million MAUs above the first two million, bringing your quota to a maximum of 240 requests per second\.

### Amazon Cognito identity pools \(federated identities\) API operation request rate quotas<a name="amazon-cognito-identity-pools-federated-identities-request-rate-quotas"></a>


| Operation | Description | Quota in requests per second | Adjustable | Quota increase eligibility | 
| --- | --- | --- | --- | --- | 
| GetId | Retrieve an identity ID from an identity pool\. | 25 | Yes | Contact your account team\. | 
| GetOpenIdToken | Retrieve an OpenID token from an identity pool in the classic workflow\. | 200 | Yes | Contact your account team\. | 
| GetCredentialsForIdentity | Retrieve AWS credentials from an identity pool in the enhanced workflow\. | 200 | Yes | Contact your account team\. | 
| GetOpenIdTokenForDeveloperIdentity | Retrieve an OpenID token from an identity pool in the developer workflow\. | 200 | Yes | Contact your account team\. | 

## Resource quotas<a name="resource-quotas"></a>

Resource quotas define the maximum number and size of your resources\. You can request to increase adjustable resource quotas in Amazon Cognito\. To request a quota increase, use the Service Quotas console or the [Service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\. To request a quota from the Service Quotas console, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. If the quota isn't available in Service Quotas, use the [Service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\.

**Amazon Cognito user pools resource quotas **


| Resource | Quota | Adjustable | Maximum quota | 
| --- | --- | --- | --- | 
| App clients per user pool | 1,000 | Yes | 10,000 | 
| User pools per account | 1,000 | Yes | 10,000 | 
| Identity providers per user pool | 300 | Yes | 1,000 | 
| Resource servers per user pool | 25 | Yes | 300 | 
| Users per user pool | 40,000,000 | Yes | Contact your account team\. | 
| Custom attributes per user pool | 50 | No | N/A | 
| Characters per attribute | 2,048 bytes | No | N/A | 
| Characters in custom attribute name | 20 | No | N/A | 
| Required minimum password characters in password policy | 6–99 | No | N/A | 
| Email messages sent daily per AWS account ¹ | 50 | No | N/A | 
| Characters in email subject | 140 | No | N/A | 
| Characters in email message | 20,000 | No | N/A | 
| Characters in SMS verification message | 140 | No | N/A | 
| Characters in password | 256 | No | N/A | 
| Characters in identity provider name | 40 | No | N/A | 
| Identifiers per identity provider | 50 | No | N/A | 
| Identities linked to a user | 5 | No | N/A | 
| Callback URLs per app client | 100 | No | N/A | 
| Logout URLs per app client | 100 | No | N/A | 
| Scopes per resource server | 100 | No | N/A | 
| Scopes per app client | 50 | No | N/A | 
| Custom domains per account | 4 | No | N/A | 
| Groups to which each user can belong | 100 | No | N/A | 
| Groups per user pool | 10,000 | No | N/A | 

¹ This quota applies only if you are using the default email feature for an Amazon Cognito user pool\. To enable a higher email delivery volume, configure your user pool to use your Amazon SES email configuration\. For more information, see [Email settings for Amazon Cognito user pools](user-pool-email.md)\.

**Amazon Cognito user pools session validity parameters**


| Token | Quota | 
| --- | --- | 
| ID token | 5 minutes – 1 day | 
| Refresh token | 1 hour – 3,650 days | 
| Access token | 5 minutes – 1 day | 
| Hosted UI session cookie | 1 hour | 
| Authentication session token | 3 minutes – 15 minutes | 

**Amazon Cognito user pools code security resource quotas \(non\-adjustable\)**


| Resource | Quota | 
| --- | --- | 
| Sign\-up confirmation code validity period | 24 hours | 
| User attribute verification code validity period | 24 hours | 
| Multi\-factor authentication \(MFA\) code validity period | 3–15 minutes | 
| Forgot password code validity period | 1 hour | 
| Maximum number of ConfirmForgotPassword calls per user | 5–20 attempts per hour, depending on risk score | 
| Maximum number of ResendConfirmationCode calls per user | 5 attempts per hour | 
| Maximum number of ConfirmSignUp calls per user | 15 attempts per hour | 
| Maximum number of ChangePassword calls per user | 5 attempts per hour | 
| Maximum number of GetUserAttributeVerificationCode calls per user | 5 attempts per hour | 
| Maximum number of VerifyUserAttribute calls per user | 15 attempts per hour | 

**Amazon Cognito user pools user import job resource quotas \(non\-adjustable\)**


| Resource | Quota | 
| --- | --- | 
| User import jobs per user pool | 1,000 | 
| Maximum characters per user import CSV row | 16,000 | 
| Maximum CSV file size | 100 MB | 
| Maximum number of users per CSV file | 500,000 | 

**Amazon Cognito identity pools \(federated identities\) resource quotas **


| Resource | Quota | Adjustable | Maximum quota | 
| --- | --- | --- | --- | 
| Identity pools per account | 1,000 | No | N/A | 
| Amazon Cognito user pool providers per identity pool | 50 | Yes | 1000 | 
| Character length of an identity pool name | 128 bytes | No | N/A | 
| Character length of a login provider name | 2,048 bytes | No | N/A | 
| Identities per identity pool | Unlimited | No | N/A | 
| Identity providers for which role mappings can be specified  | 10 | No | N/A | 
| Results from a single list or lookup call | 60 | No | N/A | 
| Role\-based access control \(RBAC\) rules | 25 | No | N/A | 

**Amazon Cognito Sync resource quotas**


| Resource | Quota | Adjustable | Maximum quota | 
| --- | --- | --- | --- | 
| Datasets per identity | 20 | Yes | Contact your account team\. | 
| Records per dataset | 1,024 | Yes | Contact your account team\. | 
| Size of a single dataset | 1 MB | Yes | Contact your account team\. | 
| Characters in dataset name | 128 bytes | No | N/A | 
| Waiting time for a bulk publish after a successful request | 24 hours | No | N/A | 