# Quotas in Amazon Cognito<a name="limits"></a>

Amazon Cognito limits the number of operations that you can perform in your account\. Amazon Cognito also limits the number and size of Amazon Cognito resources\.

**[Operation quotas](#operation-quotas)**
+ [Quota categorization](#quota-categorization)
+ [Operation special handling](#api-operation-special-handling)
+ [Category operations](#category_operations)
+ [Track quota usage](#track-quota-usage)
+ [Optimize quotas](#optimize-quotas)
+ [Identify quota requirements](#identify-quota-requirements)
+ [API request rate quotas](#api-request-rate-quotas)

**[Resource quotas](#resource-quotas)**

## Operation quotas<a name="operation-quotas"></a>

## Quota categorization<a name="quota-categorization"></a>

Amazon Cognito limits the number of operations, such as `InitiateAuth` or `RespondToAuthChallenge`, that you can use to perform certain user actions in your applications\. These operations are grouped into categories of common use cases, such as `UserAuthentication` or `UserCreation`\. For a list of categorized operations, see [Category operations](#category_operations)\. The categories make it easier to monitor usage and request quota increases\.

 Operation quotas are defined as the number of allowed requests per second \(RPS\) for all operations within a category\. The Amazon Cognito user pools service applies quotas to all operations in each category\. For example, the category `UserCreation`, includes four operations, `SignUp`, `ConfirmSignUp`, `AdminCreateUser`, and `AdminConfirmSignUp`\. It's allocated with a combined quota of 50 RPS\. Each operation within this category can call up to 50 RPS separately or combined if multiple operations take place at the same time\. 

**Note**  
The category quota is enforced for each AWS account across all user pools in an account and region\.

## Operation special handling<a name="api-operation-special-handling"></a>

The operation quotas are measured and enforced for the combined total requests at the category level, with the exception of the `AdminRespondToAuthChallenge` and the `RespondToAuthChallenge` operations, where special handling rules are applied\. 

The `UserAuthentication` category include four operations, `AdminInitiateAuth`, `InitiateAuth`, `AdminRespondToAuthChallenge`, and `RespondToAuthChallenge`\. The `InitiateAuth` and `AdminInitiateAuth` operations are measured and enforced per category quota\. The matching `RespondToAuthChallenge` and `AdminRespondToAuthChallenge` operations are subject to a separate quota that is three times the `UserAuthentication` category limit, to accommodate multiple authentication challenges set up in developers’ apps\. This elevated quota is sufficient to cover the large majority of use cases\. Beyond the three\-time per authentication call threshold, the excess rate will be counted towards the `UserAuthentication` category limit\.

For example, if your quota for the `UserAuthentication` category is 80 RPS, you can call `RespondToAuthChallenge` or `AdminRespondToAuthChallenge` up to 240 RPS \(80 RPS x 3\)\. If the app is set up to have four rounds of challenge per authentication call and you have 70 sign\-ins per second, then total `RespondToAuthChallenge` will be 280 RPS \(70 x 4\), which is 40 RPS above the quota\. The extra 40 RPS will be added to 70 `InitiateAuth` calls\. The total usage of `UserAuthentication` category is 110 RPS \(40 \+ 70\)\. This exceeds the category quota set at 80 RPS so this app would get throttled because it's 30 RPS above the category quota of 80 RPS\.

## Category operations<a name="category_operations"></a>

You can find the mappings between operations and their respective categories in the following table\. Only adjustable category limits can be increased on customer request\. For more information, see [API request rate quotas](#api-request-rate-quotas)\. Adjustable quotas are applied at the account level\. There are some category operations that are subject to more constrained quota rules with a lower limit threshold at the user pool level\. You can find these categories with an asterisk in the table and their quotas in the note below the table\.

**Note**  
The rate limit for each category depends on Monthly Active Users \(MAUs\)\. The default limits support up to two million MAUs\. It's not recommended to submit a limit increase request if you have less than one million MAUs\. 

Category operation quotas, are applied across all users in a user pool\. There are also per\-user quotas that apply to each user\. For each user, you can make up to ten requests per second that are "read" operations including signing in or getting profile or device information\. You can also make up to ten requests per second that are "write" operations, including updating profile information or MFA settings\. You can't change per\-user quotas\.


| Category name | Operations | Description | Default quota \(in requests per second\) | Adjustable | 
| --- | --- | --- | --- | --- | 
| UserAuthentication |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that authenticate \(sign\-in\) a user\. These operations are subject to [Operation special handling](#api-operation-special-handling)\. | 120 | Yes | 
| UserCreation |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that create or confirm a native Amazon Cognito user\. Native users are users who are created and verified directly by your Cognito user pools\.  | 50 | Yes | 
| UserFederation | Amazon Cognito managed operations to federate a user via a third party IdP into Cognito\. | Operations that federate \(authenticate\) users with a third\-party identity provider into your Cognito user pools\. | 25 | Yes | 
| UserAccountRecovery |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that recover a user's account or change or update a user's password\. | 30 | No | 
| UserRead |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that retrieve a user from your user pools\.  | 120 | Yes | 
| UserUpdate |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  |  The operations that customers use to manage users and user attributes,  | 25 | No | 
| UserResourceRead |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that retrieve user resource information from Amazon Cognito such as device or group\. | 50 | Yes | 
| UserResourceUpdate |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that update user resource information, such as group\. | 25 | No | 
| UserList |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that return a list of users\. | 30 | No | 
| UserPoolRead |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that read your user pools\. | 15 | No | 
| UserPoolUpdate |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that create, update, and delete your user pools\. | 15 | No | 
| UserPoolResourceRead |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that retrieve a resource from a user pool, such as group\.\* | 20 | No | 
| UserPoolResourceUpdate |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that update resource information for a user pool, such as group\.\* | 15 | No | 
|  `UserPoolClientRead`  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that list your user pool clients\.\* | 15 | No | 
| UserPoolClientUpdate |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that create, update, and delete your user pool clients\.\* | 15 | No | 
| UserToken |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/limits.html)  | Operations that create, update, and delete your tokens\. | 120 | No | 

\*Any individual operation in this category has a constraint that prevents the operation from being called at a rate higher than 5 RPS for a single user pool\.

## Track quota usage<a name="track-quota-usage"></a>

Each API category has both `CallCount` and `ThrottleCount` CloudWatch metrics which are applied at the account level\. You can use `CallCount` to track the total number of calls customers made related to a category\. You can use `ThrottleCount` to track the total number of throttled calls related to a category\. You can use the `CallCount` and `ThottleCount` metrics with the `Sum` statistic to count the total number of calls in a category\. For more information on the CloudWatch usage metrics, see [CloudWatch usage metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)\.

Utilization and usage are two terms that may help you understand service quota monitoring\. Utilization is the percentage of a service quota in use\. For example, if the quota value is 200 resources and 150 resources are in use, the utilization is 75%\. Usage is the number of resources or operations in use for a service quota\.

**Tracking usage through CloudWatch metrics**

You can track and collect Amazon Cognito user pools utilization metrics using CloudWatch\. The CloudWatch dashboard will display metrics about every AWS service you use\. You can use CloudWatch to create metric alarms\. The alarms can be setup to send you notifications or make a change to a specific resource that you are monitoring\. For more information on CloudWatch metrics, For more information on Service Quotas, see [Track your CloudWatch usage metrics](tracking-quotas-and-usage-in-cloud-watch-and-service-quotas.md)\.

**Tracking utilization through Service Quotas metrics**

Amazon Cognito user pools is integrated with Service Quotas, which is a browser\-based interface that you can use to view and manage your service quota usage\. In the Service Quotas console, you can look up the value of a specific quota, view monitoring information, request a quota increase, and setup CloudWatch alarms\. After your account has been active a while, you can view a graph of your resource utilization\.

For more information on viewing quotas in the Service Quotas console, see [Viewing Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/gs-request-quota.html)\. 

## Identify quota requirements<a name="identify-quota-requirements"></a>

**Important**  
If you increase Amazon Cognito quotas for categories such as `UserAuthentication`, `UserCreation`, and `AccountRecovery`, you may need to increase quotas for other services\. For example, messages that Amazon Cognito sends with Amazon Simple Notification Service and Amazon Simple Email Service can fail if request rate quotas are insufficient in those services\.

To calculate quota requirements, determine how many active users will interact with your application in a specific time period\. For example, if your application expects an average of 1 million active users to sign\-in within an 8\-hour period, then you would need to be able to authenticate an average of 35 users per second\. 

In addition, if you assume the average user session is two hours, and you configure tokens to expire after an hour, each user has to refresh their tokens once during their session\. Then, the required average quota for the `UserAuthentication` category to support this load is 70 RPS\.

If you assume a peak\-to\-average ratio of 3:1 by accounting for the variance of user sign\-in frequency during the 8\-hour period, then you need the desired `UserAuthentication` quota of 200 RPS\. 

**Note**  
If you call multiple operations for each user action, you need to sum up the individual operation call rates at the category level\.

## Optimize quotas<a name="optimize-quotas"></a>

Follow one of the approached below to handle a peak call rate\. 

**Retry the attempt after a back\-off waiting period**  
You can catch the error at every API call, and then re\-try the attempt after a back\-off period\. You can adjust the back\-off algorithm according to business needs and load\. Amazon SDKs have built\-in retry logic\. For more information on Amazon Tools and SDKs, see [ Tools to Build on AWS\.](https://aws.amazon.com/tools/ ) 

**Use a external database for frequently updated attributes**  
If your application requires several calls to a user pool to read or write custom attributes, use external storage\. You can use your preferred database to store custom attributes or use a cache layer to load a user profile during sign\-in\. You can reference this profile from the cache when needed instead of reloading the user profile from a user pool\.

**Validate JWT tokens on the client\-side**  
Applications have to validate JWT tokens before trusting them\. You can verify the signature and validity of tokens on the client side without sending API calls to a user pool\. After the token is validated, you can trust claims in the token and use the claims instead of making more `getUser` API calls\. For more information on JSON token verification, see [ Verifying a JSON Web Token\.](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-verifying-a-jwt.html)

## API request rate quotas<a name="api-request-rate-quotas"></a>

Amazon Cognito limits the number of user pool operations that you can perform in your account\. You can request an increase to the adjustable API request rate quotas in Amazon Cognito\. To request a quota increase, you can use the Service Quotas console, use the Service limit increase form, use the `RequestServiceQuotaIncrease` API or the `ListAWSDefaultServiceQuotas` API\.
+ To request a quota increase using the Service Quotas console, see [Requesting a API quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\.
+ If the quota is not yet available in Service Quotas, use the [Service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\.

**Note**  
Only adjustable quotas can be increased\.

## Resource quotas<a name="resource-quotas"></a>

Resource quotas are quotas that limit the number and size of your resources\. You can request an increase to the adjustable resource quotas in Amazon Cognito\. To request a quota increase, you can use the Service Quotas console or use the [Service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\. To request a quota from the Service Quotas console, see [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. If the quota is not yet available in Service Quotas, use the [Service limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\.

### Adjustable resource quotas<a name="limits-soft"></a>

 

The following tables provide the adjustable resource quotas for Amazon Cognito\. These quotas can be changed\.

**User pools resource quotas **


| Resource | Quota | 
| --- | --- | 
| Maximum number of app clients per user pool | 1,000 | 
| Maximum number of user pools per account | 1,000 | 
| Maximum number of user import jobs per user pool | 1,000 | 
| Maximum number of identity providers per user pool | 300 | 
| Maximum number of resource servers per user pool | 25 | 
| Maximum number of users per user pool | 40,000,000 | 

**User pools hosted UI request rate quotas**


| Request rate | Quota \(requests per second\) | 
| --- | --- | 
| Maximum requests per app client | 300 | 
| Maximum requests per user pool domain | 500 | 
| Maximum requests per IP address | 300 | 

**Identity pools \(federated users\) resource quotas **


| Resource | Quota | 
| --- | --- | 
| Maximum number of identity pools per account | 1,000 | 
| Maximum Amazon Cognito user pool providers per identity pool | 50 | 

**Sync resource quotas**


| Resource | Quota | 
| --- | --- | 
| Maximum number of datasets per identity | 20 | 
| Maximum number of records per dataset | 1,024 | 
| Maximum size of a single dataset | 1 MB | 

### Non\-adjustable resource quotas<a name="limits-hard"></a>

The following tables describe Amazon Cognito non\-adjustable quotas\. These quotas cannot be changed\.

**User pools token validity quotas **


| Token | Quota | 
| --- | --- | 
| ID token | 5 minutes – 1 day | 
| Refresh token | 1 hour – 3,650 days | 
| Access token | 5 minutes – 1 day | 

**User pools resource quotas**


| Resource | Quota | 
| --- | --- | 
| Maximum number of custom attributes per user pool | 50 | 
| Maximum characters per attribute | 2,048 bytes | 
| Maximum characters in custom attribute name | 20 | 
| Minimum and maximum characters in password  | 6 – 99 | 
| Maximum number of email messages sent daily per AWS account ¹ | 50 | 
| Maximum characters in email subject | 140 | 
| Maximum characters in email message | 20,000 | 
| Maximum characters in SMS verification message | 140 | 
| Maximum characters in password | 256 | 
| Maximum characters in identity provider name | 40 | 
| Maximum identifiers per identity provider | 50 | 
| Maximum number of identities linked to a user | 5 | 
| Maximum callback URLs per app client | 100 | 
| Maximum logout URLs per app client | 100 | 
| Maximum number of scopes per resource server | 100 | 
| Maximum number of scopes per app client | 50 | 
| Maximum number of custom domains per account | 4 | 
| Maximum number of groups that each user can belong to | 100 | 
| Maximum number of groups per user pool | 10,000 | 

¹This quota applies only if you are using the default email feature for an Amazon Cognito User Pool\. To enable a higher email delivery volume, configure your user pool to use your Amazon SES email configuration\. For more information, see [Email settings for Amazon Cognito user pools](user-pool-email.md)\.

**User pools code validity resource quotas**


| Resource | Quota | 
| --- | --- | 
| Sign\-up confirmation code | 24 hours | 
| User attribute verification code validity | 24 hours | 
| Multi\-factor authentication code | 3 minutes | 
| Forgot password code | 1 hour | 

**User pools hosted UI request rate quotas**


| Request rate | Quota \(requests per second\) | 
| --- | --- | 
| Maximum client credentials requests | 150 | 

**Identity pools \(federated users\) resource quotas**


| Resource | Quota | 
| --- | --- | 
| Maximum character length for identity pool name | 128 bytes | 
| Maximum character length for login provider name | 2,048 bytes | 
| Maximum number of identities per identity pool | Unlimited | 
| Maximum number of identity providers for which role mappings can be specified  | 10 | 
| Maximum number of results from a single list or lookup call | 60 | 
| Maximum number of role\-based access control \(RBAC\) rules | 25 | 

**Sync resource quotas**


| Resource | Quota | 
| --- | --- | 
| Maximum characters in dataset name | 128 bytes | 
| Minimum waiting time for a bulk publish after a successful request | 24 hours | 