# Dimensions for Amazon Cognito user pools<a name="dimensions-for-cognito-user-pools"></a>

The following dimensions are used to refine the usage metrics that are published by Amazon Cognito\. The dimensions only apply to `CallCount` and `ThrottleCount ` metrics\.


| Dimension | Description | 
| --- | --- | 
|  Service  |  The name of the AWS service containing the resource\. For Amazon Cognito usage metrics, the value for this dimension is `Cognito user pool`\.  | 
|  Type  |  The type of entity that is being reported\. The only valid value for Amazon Cognito usage metrics is API\.  | 
|  Resource  |  The type of resource that is running\. The only valid value is category name\.   | 
|  Class  |  The class of resource being tracked\. Amazon Cognito doesn't use the class dimension\.  | 