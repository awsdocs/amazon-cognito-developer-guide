# Use the CloudWatch console to track metrics<a name="use-the-cloud-watch-console-to-track-metrics"></a>

You can track and collect Amazon Cognito user pools metrics using CloudWatch\. The CloudWatch dashboard will display metrics about every AWS service you use\. You can use CloudWatch to create metric alarms\. The alarms can be setup to send you notifications or make a change to a specific resource that you are monitoring\. To view service quota metrics in CloudWatch, complete the following steps\.

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In **All metrics** select a metric and a dimension\.

1. Select the check box next to a metric\. The metrics will appear in the graph\.

**Note**  
Metrics that haven't had any new data points in the past two weeks don't appear in the console\. They also don't appear when you enter their metric name or dimension names in the search box in the All metrics tab in the console, and they are not returned in the results of a list\-metrics command\. The best way to retrieve these metrics is with the `get-metric-data` or `get-metric-statistics` commands in the AWS CLI\.