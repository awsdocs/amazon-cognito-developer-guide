# Use the Service Quotas console to track metrics<a name="use-the-service-quota-console-to-track-metrics"></a>

You can view and manage your Amazon Cognito user pools quotas from a central location with Service Quotas\. You can use the Service Quotas console to view details about a specific quota, monitor quota utilization, and request a quota increase\. For some quota types, you can create a CloudWatch alarm to track your quota utilization\.To learn more about what Amazon Cognito metrics you can track, see [Track quota usage](limits.md#track-quota-usage)\.

**To view Amazon Cognito user pools service quotas utilization, complete the following steps\.**

1. Open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\.

1. In the navigation pane, choose **AWS services**\.

1. From the **AWS services** list, enter Amazon Cognito user pools in the search field\. The service quota page appears\. 

1. Select a quota that supports CloudWatch monitoring\. For example, choose `Rate of UserAuthentication requests`\.

1. Scroll down to **Monitoring**\. This section appears only for quotas that support CloudWatch monitoring\.

1. In **Monitoring** you can view current service quota utilization in the graph\.

1. In **Monitoring** select either one hour, three hours, twelve hours, one day, three days, or one week\.

1. Select any area inside of the graph to view the service quota utilization percentage\. From here, you can add the graph to your dashboard or use the action menu to select **View in metrics**, which will take you to the related metrics in the CloudWatch console\.