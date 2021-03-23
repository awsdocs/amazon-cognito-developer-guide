# Create a CloudWatch alarm for a quota<a name="create-a-cloud-watch-alarm"></a>

 Amazon Cognito provides CloudWatch usage metrics that correspond to the AWS service quotas for `CallCount` and `ThrottleCount` APIs\. In the Service Quotas console, you can create alarms that alert you when your usage approaches a service quota\. Use the following steps to set up a CloudWatch alarm using the Service Quotas console\. 

1. Open the Service Quota https://docs\.aws\.amazon\.com/servicequotas/latest/userguide/ console\.

1. In the navigation pane, choose **AWS services**\.

1. Select **Amazon Cognito user pools**\.

1. Select the quota you want to setup an alarm on\.

1. Scroll down to **CloudWatch alarms**\.

1. In **CloudWatch alarms**, choose**Create**,

1. In **Alarm threshold**, choose a threshold\.

1. For **Alarm name**, enter a unique name\.

1. Choose **Create**\.