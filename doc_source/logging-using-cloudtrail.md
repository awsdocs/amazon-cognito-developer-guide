# Logging Amazon Cognito API calls with AWS CloudTrail<a name="logging-using-cloudtrail"></a>

Amazon Cognito is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Cognito\. CloudTrail captures a subset of API calls for Amazon Cognito as events, including calls from the Amazon Cognito console and from code calls to the Amazon Cognito API operations\. If you create a trail, you can choose to deliver CloudTrail events to an Amazon S3 bucket, including events for Amazon Cognito\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Cognito, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and activate it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

You can also create Amazon CloudWatch alarms for specific CloudTrail events\. For example, you can set up CloudWatch to trigger an alarm if an identity pool configuration is changed\. For more information, see [Creating CloudWatch alarms for CloudTrail events: Examples](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)\.

**Topics**
+ [Amazon Cognito information in CloudTrail](amazon-cognito-info-in-cloudtrail.md)
+ [Understanding Amazon Cognito sign\-in events](understanding-amazon-cognito-entries.md)
+ [Analyzing Amazon Cognito CloudTrail events with Amazon CloudWatch Logs Insights](analyzingcteventscwinsight.md)