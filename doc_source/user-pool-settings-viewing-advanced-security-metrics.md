# Viewing advanced security metrics<a name="user-pool-settings-viewing-advanced-security-metrics"></a>

Amazon Cognito publishes metrics for advanced security features to your account in Amazon CloudWatch\. Amazon Cognito groups the advanced security metrics together by risk level and also by request level\.

**To view metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose Amazon Cognito\.

1. Choose a group of aggregated metrics, such as **By Risk Classification**\. 

1. The **All metrics** tab displays all metrics for that choice\. You can do the following:
   + To sort the table, use the column heading\.
   + To graph a metric, select the check box next to the metric\. To select all metrics, select the check box in the heading row of the table\.
   + To filter by resource, choose the resource ID, and then choose **Add to search**\.
   + To filter by metric, choose the metric name, and then choose **Add to search**\.


| Metric | Description | Metric Dimensions | 
| --- | --- | --- | 
| CompromisedCredentialRisk | Requests where Amazon Cognito detected compromised credentials\. |  Operation: The type of operation\. `PasswordChange`, `SignIn`, or `SignUp` are the only dimensions\. UserPoolId: The identifier of the user pool\. RiskLevel: high \(default\), medium, or low\.  | 
| AccountTakeOverRisk | Requests where Amazon Cognito detected account take\-over risk\. |  Operation: The type of operation\. `PasswordChange`, `SignIn`, or `SignUp` are the only dimensions\. UserPoolId: The identifier of the user pool\. RiskLevel: high, medium, or low\. | 
| OverrideBlock | Requests that Amazon Cognito blocked because of the configuration provided by the developer\. |  Operation: The type of operation\. `PasswordChange`, `SignIn`, or `SignUp` are the only dimensions\. UserPoolId: The identifier of the user pool\. RiskLevel: high, medium, or low\. | 
| Risk | Requests that Amazon Cognito marked as risky\. | Operation: The type of operation, such as `PasswordChange`, `SignIn`, or `SignUp`\. UserPoolId: The identifier of the user pool\. | 
| NoRisk | Requests where Amazon Cognito did not identify any risk\.  | Operation: The type of operation, such as `PasswordChange`, `SignIn`, or `SignUp`\. UserPoolId: The identifier of the user pool\. | 

Amazon Cognito offers you two predefined groups of metrics for ready analysis in CloudWatch\. **By Risk Classification** identifies the granularity of the risk level for requests that Amazon Cognito identifies as risky\. **By Request Classification** reflects metrics aggregated by request level\.


| Aggregated Metrics Group | Description | 
| --- | --- | 
| By Risk Classification | Requests that Amazon Cognito identifies as risky\. | 
| By Request Classification | Metrics aggregated by request\. | 