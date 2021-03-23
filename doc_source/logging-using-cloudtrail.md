# Logging Amazon Cognito API Calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon Cognito is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Cognito\. CloudTrail captures a subset of API calls for Amazon Cognito as events, including calls from the Amazon Cognito console and from code calls to the Amazon Cognito API operations\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon Cognito\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Cognito, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

You can also create Amazon CloudWatch alarms for specific CloudTrail events\. For example, you can set up CloudWatch to trigger an alarm if an identity pool configuration is changed\. For more information, see [Creating CloudWatch Alarms for CloudTrail Events: Examples](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)\.

## Amazon Cognito Information in CloudTrail<a name="amazon-cognito-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When supported event activity occurs in Amazon Cognito, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\.

For an ongoing record of events in your AWS account, including events for Amazon Cognito, create a trail\. A trail enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-list)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

**Amazon Cognito User Pools**

Amazon Cognito supports logging for all of the actions listed on the [User Pool Actions](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_Operations.html) page as events in CloudTrail log files\. Amazon Cognito records `UserSub` but not `UserName` in CloudTrail logs for requests that are specific to a user\. You can find a user for a given `UserSub` by calling the `ListUsers` API, and using a filter for sub\. 

**Note**  
Hosted UI and federation calls are currently not included in CloudTrail\.

**Amazon Cognito Federated Identities**
+ [CreateIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_CreateIdentityPool.html)
+ [DeleteIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_DeleteIdentityPool.html)
+ [DescribeIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_DescribeIdentityPool.html)
+ [GetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetIdentityPoolRoles.html)
+ [ListIdentityPools](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_ListIdentityPools.html)
+ [SetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_SetIdentityPoolRoles.html)
+ [UpdateIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_UpdateIdentityPool.html)

**Amazon Cognito Sync**
+ [BulkPublish](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_BulkPublish.html)
+ [DescribeIdentityPoolUsage](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_DescribeIdentityPoolUsage.html)
+ [GetBulkPublishDetails](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_GetBulkPublishDetails.html)
+ [GetCognitoEvents](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_GetCognitoEvents.html)
+ [GetIdentityPoolConfiguration](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_GetIdentityPoolConfiguration.html)
+ [ListIdentityPoolUsage](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_ListIdentityPoolUsage.html)
+ [SetCognitoEvents](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_SetCognitoEvents.html)
+ [SetIdentityPoolConfiguration](https://docs.aws.amazon.com/cognitosync/latest/APIReference/API_SetIdentityPoolConfiguration.html)

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Example: Amazon Cognito Log File Entries<a name="understanding-amazon-cognito-entries"></a>

 A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\.

The following example is a log entry for a request for the `CreateIdentityPool` action\. The request was made by an IAM user named Alice\. 

```
{
         "eventVersion":"1.03",
         "userIdentity":{
            "type":"IAMUser",
            "principalId":"PRINCIPAL_ID",
            "arn":"arn:aws:iam::123456789012:user/Alice",
            "accountId":"123456789012",
            "accessKeyId":"['EXAMPLE_KEY_ID']",
            "userName":"Alice"
         },
         "eventTime":"2016-01-07T02:04:30Z",
         "eventSource":"cognito-identity.amazonaws.com",
         "eventName":"CreateIdentityPool",
         "awsRegion":"us-east-1",
         "sourceIPAddress":"127.0.0.1",
         "userAgent":"USER_AGENT",
         "requestParameters":{
            "identityPoolName":"TestPool",
            "allowUnauthenticatedIdentities":true,
            "supportedLoginProviders":{
               "graph.facebook.com":"000000000000000"
            }
         },
         "responseElements":{
            "identityPoolName":"TestPool",
            "identityPoolId":"us-east-1:1cf667a2-49a6-454b-9e45-23199EXAMPLE",
            "allowUnauthenticatedIdentities":true,
            "supportedLoginProviders":{
               "graph.facebook.com":"000000000000000"
            }
         },
         "requestID":"15cc73a1-0780-460c-91e8-e12ef034e116",
         "eventID":"f1d47f93-c708-495b-bff1-cb935a6064b2",
         "eventType":"AwsApiCall",
         "recipientAccountId":"123456789012"
      }
```