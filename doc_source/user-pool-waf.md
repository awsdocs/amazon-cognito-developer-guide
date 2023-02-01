# Associating an AWS WAF web ACL with a user pool<a name="user-pool-waf"></a>

AWS WAF is a web application firewall\. With an AWS WAF web access control list \(web ACL\), you can protect your user pool from unwanted requests to your hosted UI and Amazon Cognito API service endpoints\. A web ACL gives you fine\-grained control over all of the HTTPS web requests that your user pool responds to\. For more information about AWS WAF web ACLs, see [Managing and using a web access control list \(web ACL\)](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html) in the *AWS WAF Developer Guide*\.

When you have an AWS WAF web ACL associated with a user pool, Amazon Cognito forwards selected non\-confidential headers and contents of requests from your users to AWS WAF\. AWS WAF inspects the contents of the request, compares it to the rules that you specified in your web ACL, and returns a response to Amazon Cognito\.

## Things to know about AWS WAF web ACLs and Amazon Cognito<a name="user-pool-waf-things-to-know"></a>
+ When you create a web ACL, a small amount of time passes before the web ACL has fully propagated and is available to Amazon Cognito\. The propagation time can be from a few seconds to a number of minutes\. AWS WAF returns a [https://docs.aws.amazon.com/waf/latest/APIReference/API_AssociateWebACL.html#API_AssociateWebACL_Errors](https://docs.aws.amazon.com/waf/latest/APIReference/API_AssociateWebACL.html#API_AssociateWebACL_Errors) when you attempt to associate a web ACL before it has fully propagated\.
+ You can associate one web ACL with a user pool\.
+ Your request might result in a payload that is larger than the limits of what AWS WAF can inspect\. See [Oversize request component handling](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-oversize-handling.html) in the *AWS WAF Developer Guide* to learn how to configure how AWS WAF handles oversize requests from Amazon Cognito\.
+ You can’t associate a web ACL that uses AWS WAF [Fraud Control account takeover prevention \(ATP\)](https://docs.aws.amazon.com/waf/latest/developerguide/waf-atp.html) with an Amazon Cognito user pool\. You implement the ATP feature when you add the `AWS-AWSManagedRulesATPRuleSet` managed rule group\. Before you associate it with a user pool, ensure that your web ACL doesn’t use this managed rule group\.
+ When you have an AWS WAF web ACL associated with a user pool, and a rule in your web ACL presents a CAPTCHA, this can cause an unrecoverable error in hosted UI TOTP registration\. To create a rule that has a CAPTCHA action and doesn't affect hosted UI TOTP, see [Configuring your AWS WAF web ACL for hosted UI TOTP MFA](user-pool-settings-mfa-totp.md#hosted-ui-totp-waf)\.

AWS WAF inspects requests to the following endpoints\.

**Hosted UI**  
Requests to all endpoints in the [User pool OIDC and hosted UI API endpoints reference](cognito-userpools-server-contract-reference.md)\.

**Public API operations**  
Requests from your app to the Amazon Cognito API that don't use AWS credentials to authorize\. This includes API operations like [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html), [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html), and [GetUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html)\. The API operations that are in scope of AWS WAF don't require authentication with AWS credentials\. They are unauthenticated, or authorized with a session string or access token\. For more information, see [Amazon Cognito user pools authenticated and unauthenticated API operations](user-pools-API-operations.md#user-pool-apis-auth-unauth)\.

You can configure the rules in your web ACL with rule actions that **Count**, **Allow**, **Block**, or present a **CAPTCHA** in response to a request that matches a rule\. For more information, see [AWS WAF rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) in the *AWS WAF Developer Guide*\. Depending on the rule action, you can customize the response that Amazon Cognito returns to your users\.

**Important**  
Your options to customize the error response depends on the way you make an API request\.  
You can customize the error code and response body of hosted UI requests\. You can only present a CAPTCHA for your user to solve in the hosted UI\.
For requests that you make with the Amazon Cognito[ native API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html), you can customize the response body of a request that receives a **Block** response\. You can also specify a custom error code in the range 400–499\.
The AWS Command Line Interface \(AWS CLI\) and the AWS SDKs return a `ForbiddenException` error to requests that produce a **Block** or **CAPTCHA** response\.

## Associating a web ACL with your user pool<a name="user-pool-waf-setting-up"></a>

To work with a web ACL in your user pool, your AWS Identity and Access Management \(IAM\) principal must have the following Amazon Cognito permissions\. For information about AWS WAF permissions, see [AWS WAF API permissions](https://docs.aws.amazon.com/waf/latest/developerguide/waf-api-permissions-ref.html) in the *AWS WAF Developer Guide*\.
+ `cognito-idp:AssociateWebACL`
+ `cognito-idp:DisassociateWebACL`
+ `cognito-idp:GetWebACLForResource`
+ `cognito-idp:ListResourcesForWebACL`

Though you must grant IAM permissions, the listed actions are permission\-only and don't correspond to an [API operation](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html)\.

## To activate AWS WAF for your user pool and associate a web ACL

1. Sign in to the [Amazon Cognito console ](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, choose **User Pools**, and choose the user pool you want to edit\.

1. Choose the **User pool properties** tab\.

1. Choose **Edit** next to **AWS WAF**\.

1. Under **AWS WAF**, select **Use AWS WAF with your user pool**\.  
![\[Screenshot of the AWS WAF dialog box with Use AWS WAF with your user pool selected.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Screenshot of the AWS WAF dialog box with Use AWS WAF with your user pool selected.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Screenshot of the AWS WAF dialog box with Use AWS WAF with your user pool selected.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. Choose an **AWS WAF Web ACL** that you already created, or choose **Create web ACL in AWS WAF** to create one in a new AWS WAF session in the AWS Management Console\.

1. Choose **Save changes**\.

To programmatically associate a web ACL with your user pool in the AWS Command Line Interface or an SDK, use [AssociateWebACL](https://docs.aws.amazon.com/waf/latest/APIReference/API_AssociateWebACL.html) from the AWS WAF API\. Amazon Cognito doesn't have a separate API operation that associates a web ACL\.

## Testing and logging AWS WAF web ACLs<a name="user-pool-waf-evaluating-and-logging"></a>

When you set a rule action to **Count** in your web ACL, AWS WAF adds the request to a count of requests that match the rule\. To test a web ACL with your user pool, set rule actions to **Count** and consider the volume of requests that match each rule\. For example, if a rule that you want to set to a **Block** action matches a large number of requests that you determine to be normal user traffic, you might need to reconfigure your rule\. For more information, see [Testing and tuning your AWS WAF protections](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing.html) in the * AWS WAF Developer Guide\.*

You can also configure AWS WAF to log request headers to an Amazon CloudWatch Logs log group, an Amazon Simple Storage Service \(Amazon S3\) bucket, or an Amazon Kinesis Data Firehose\. You can identify the Amazon Cognito requests that you make with the native API by the `x-amzn-cognito-client-id` and `x-amzn-cognito-operation-name`\. Hosted UI requests only include the `x-amzn-cognito-client-id` header\. For more information, see [Logging web ACL traffic]() in the *AWS WAF Developer Guide*\.

AWS WAF web ACLs aren't subject to the [pricing](http://aws.amazon.com/cognito/pricing) for Amazon Cognito [advanced security features](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-settings-advanced-security.html)\. The security features of AWS WAF complement Amazon Cognito advanced security features\. You can activate both features in a user pool\. AWS WAF bills separately for the inspection of user pool requests\. For more information, see [AWS WAF Pricing](http://aws.amazon.com/waf/pricing)\.

Logging AWS WAF request data is subject to additional billing by the service where you target your logs\. For more information, see [Pricing for logging web ACL traffic information](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html#logging-pricing) in the *AWS WAF Developer Guide*\.