# Logging and monitoring in Amazon Cognito<a name="monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon Cognito and your other AWS solutions\. Amazon Cognito currently supports the following two AWS services so that you can monitor your organization and the activity that happens within it\.
+ **AWS CloudTrail –** With CloudTrail you can capture API calls from the Amazon Cognito console and from code calls to the Amazon Cognito API operations\. For example, when a user authenticates, CloudTrail can record details such as the IP address in the request, who made the request, and when it was made\.
+ **Amazon CloudWatch Metrics –** With CloudWatch metrics you can monitor, report and take automatic actions in case of an event in near real time\. For example, you can create CloudWatch dashboards on the provided metrics to monitor your Amazon Cognito user pools, or you can create CloudWatch alarms on the provided metrics to notify you on breach of a set threshold\.
+ **Amazon CloudWatch Logs Insights** \- With CloudWatch Logs Insights, you can configure CloudTrail to send events to CloudWatch for monitoring Amazon Cognito CloudTrail log files\.

**Topics**
+ [Tracking quotas and usage in CloudWatch and Service Quotas](tracking-quotas-and-usage-in-cloud-watch-and-service-quotas.md)
+ [Metrics for Amazon Cognito user pools](metrics-for-cognito-user-pools.md)
+ [Dimensions for Amazon Cognito user pools](dimensions-for-cognito-user-pools.md)
+ [Use the Service Quotas console to track metrics](use-the-service-quota-console-to-track-metrics.md)
+ [Use the CloudWatch console to track metrics](use-the-cloud-watch-console-to-track-metrics.md)
+ [Create a CloudWatch alarm for a quota](create-a-cloud-watch-alarm.md)
+ [Logging Amazon Cognito API calls with AWS CloudTrail](logging-using-cloudtrail.md)
+ [Analyzing Amazon Cognito CloudTrail events with Amazon CloudWatch Logs Insights](analyzingcteventscwinsight.md)