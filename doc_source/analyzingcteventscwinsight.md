# Analyzing Amazon Cognito CloudTrail events with Amazon CloudWatch Logs Insights<a name="analyzingcteventscwinsight"></a>

Amazon CloudWatch Logs Insights enables you to interactively search and analyze your Amazon Cognito CloudTrail events data\. When you configure your trail to send events to CloudWatch Logs, CloudTrail sends only the events that match your trail settings\.

To query or research your Amazon Cognito CloudTrail events, in the CloudTrail console, make sure that you select the **Management events** option in your trail settings so that you can monitor the management operations performed on your AWS resources\. You can optionally select the **Insights events** option in your trail settings when you want to identify errors, unusual activity or unusual user behavior in your account\.

## Sample Amazon Cognito queries<a name="analyzingcteventscwinsight-samplequeries"></a>

You can use the following queries in the Amazon CloudWatch console\.

**General queries**

Find the 25 most recently added log events\.

```
fields @timestamp, @message | sort @timestamp desc | limit 25
| filter eventSource = "cognito-idp.amazonaws.com"
```

Get a list of the 25 most recently added log events that include exceptions\.

```
fields @timestamp, @message | sort @timestamp desc | limit 25
| filter eventSource = "cognito-idp.amazonaws.com" and @message like /Exception/
```

**Exception and Error Queries**

Find the 25 most recently added log events with error code `NotAuthorizedException` along with Amazon Cognito user pool `sub`\.

```
fields @timestamp, additionalEventData.sub as user | sort @timestamp desc | limit 25
| filter eventSource = "cognito-idp.amazonaws.com" and errorCode= "NotAuthorizedException"
```

Find the number of records with `sourceIPAddress` and corresponding `eventName`\.

```
filter eventSource = "cognito-idp.amazonaws.com"
| stats count(*) by sourceIPAddress, eventName
```

Find the top 25 IP addresses that triggered a `NotAuthorizedException` error\.

```
filter eventSource = "cognito-idp.amazonaws.com" and errorCode= "NotAuthorizedException"
| stats count(*) as count by sourceIPAddress, eventName
| sort count desc | limit 25
```

Find the top 25 IP addresses that called the `ForgotPassword` API\.

```
filter eventSource = "cognito-idp.amazonaws.com" and eventName = 'ForgotPassword'
| stats count(*) as count by sourceIPAddress
| sort count desc | limit 25
```