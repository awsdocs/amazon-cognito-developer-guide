# Amazon Cognito Events<a name="cognito-events"></a>

****  
If you're new to Amazon Cognito Sync, use [AWS AppSync](https://aws.amazon.com/appsync/)\. Like Amazon Cognito Sync, AWS AppSync is a service for synchronizing application data across devices\.  
It enables user data like app preferences or game state to be synchronized\. It also extends these capabilities by allowing multiple users to synchronize and collaborate in real time on shared data\.

Amazon Cognito Events allows you to execute an AWS Lambda function in response to important events in Amazon Cognito\. Amazon Cognito raises the Sync Trigger event when a dataset is synchronized\. You can use the Sync Trigger event to take an action when a user updates data\. The function can evaluate and optionally manipulate the data before it is stored in the cloud and synchronized to the user's other devices\. This is useful to validate data coming from the device before it is synchronized to the user's other devices, or to update other values in the dataset based on incoming data such as issuing an award when a player reaches a new level\.

The steps below will guide you through setting up a Lambda function that executes each time a Amazon Cognito Dataset is synchronized\.

**Note**  
When using Amazon Cognito events, you can only use the credentials obtained from Amazon Cognito Identity\. If you have an associated Lambda function, but you call `UpdateRecords` with AWS account credentials \(developer credentials\), your Lambda function will not be invoked\.

**Creating a function in AWS Lambda**

To integrate Lambda with Amazon Cognito, you first need to create a function in Lambda\. To do so:

**Selecting the Lambda Function in Amazon Cognito**

1. Open the Lambda console\.

1. Click Create a Lambda function\.

1. On the Select blueprint screen, search for and select "cognito\-sync\-trigger\."

1. On the Configure event sources screen, leave the Event source type set to "Cognito Sync Trigger" and select your identity pool\. Click Next\.
**Note**  
When configuring a Amazon Cognito Sync trigger outside of the console, you must add Lambda resource\-based permissions to allow Amazon Cognito to invoke the function\. You can add this permission from the Lambda console \(see [Using resource\-based policies for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)\) or by using the Lambda [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) operation\.  
**Example Lambda Resource\-Based Policy**  
The following AWS Lambda resource\-based policy grants Amazon Cognito a limited ability to invoke a Lambda function\. Amazon Cognito can only invoke the function on behalf of the identity pool in the `aws:SourceArn` condition and the account in the `aws:SourceAccount` condition\.  

   ```
   {
       "Version": "2012-10-17",
       "Id": "default",
       "Statement": [
           {
               "Sid": "lambda-allow-cognito-my-function",
               "Effect": "Allow",
               "Principal": {
                   "Service": "cognito-sync.amazonaws.com"
               },
               "Action": "lambda:InvokeFunction",
               "Resource": "<your Lambda function ARN>",
               "Condition": {
                   "StringEquals": {
                       "AWS:SourceAccount": "<your account number>"
                   },
                   "ArnLike": {
                       "AWS:SourceArn": "<your identity pool ARN>"
                   }
               }
           }
       ]
   }
   ```

1. On the Configure function screen, enter a name and description for your function\. Leave Runtime set to "Node\.js\." Leave the code unchanged for our example\. The default example makes no changes to the data being synced\. It only logs the fact that the Amazon Cognito Sync Trigger event occurred\. Leave Handler name set to "index\.handler\." For Role, select an IAM role that grants your code permission to access AWS Lambda\. To modify roles, see the IAM console\. Leave Advanced settings unchanged\. Click Next\.

1. On the Review screen, review the details and click Create function\. The next page displays your new Lambda function\.

Now that you have an appropriate function written in Lambda, you need to choose that function as the handler for the Amazon Cognito Sync Trigger event\. The steps below walk you through this process\.

From the console home page:

1. Click the name of the identity pool for which you want to set up Amazon Cognito Events\. The Dashboard page for your identity pool appears\.

1. In the top\-right corner of the Dashboard page, click Manage Federated Identities\. The Manage Federated Identities page appears\.

1. Scroll down and click Cognito Events to expand it\.

1. In the Sync Trigger dropdown menu, select the Lambda function that you want to trigger when a Sync event occurs\.

1. Click Save Changes\.

Now, your Lambda function will be executed each time a dataset is synchronized\. The next section explains how you can read and modify the data in your function as it is being synchronized\.

**Writing a Lambda function for sync triggers**

Sync triggers follow the service provider interface programming paradigm\. Amazon Cognito will provide input in the following JSON format to your Lambda function\.

```
{
  "version": 2,
  "eventType": "SyncTrigger",
  "region": "us-east-1",
  "identityPoolId": "identityPoolId",
  "identityId": "identityId",
  "datasetName": "datasetName",
  "datasetRecords": {
    "SampleKey1": {
      "oldValue": "oldValue1",
      "newValue": "newValue1",
      "op": "replace"
    },
    "SampleKey2": {
      "oldValue": "oldValue2",
      "newValue": "newValue2",
      "op": "replace"
    },..
  }
}
```

Amazon Cognito expects the return value of the function in the same format as the input\. A complete example is provided below\.

Some key points to keep in mind when writing functions for the Sync Trigger event:
+ When your Lambda function is invoked during UpdateRecords, it must respond within 5 seconds\. If it does not, the Amazon Cognito Sync service throws a `LambdaSocketTimeoutException` exception\. It is not possible to increase this timeout value\.
+ If you get a `LambdaThrottledException` exception, you should retry the sync operation \(update records\)\.
+ Amazon Cognito will provide all the records present in the dataset as input to the function\.
+ Records updated by the app user will have the 'op' field set as “replace” and the records deleted will have 'op' field as "remove"\.
+ You can modify any record, even if it is not updated by the app user\.
+ All the fields except the datasetRecords are read only and should not be changed\. Changing these fields will result in a failure to update the records\.
+ To modify the value of a record, simply update the value and set the 'op' to "replace"\.
+ To remove a record, either set the ‘op’ to remove or set the value to null\.
+ To add a record, simply add a new record to the datasetRecords array\.
+ Any omitted record in the response will be ignored for the update\.

**Sample Lambda function**

Here is a sample Lambda function showing how to access, modify and remove the data\.

```
console.log('Loading function');

exports.handler = function(event, context) {
    console.log(JSON.stringify(event, null, 2));

    //Check for the event type
    if (event.eventType === 'SyncTrigger') {

        //Modify value for a key
        if('SampleKey1' in event.datasetRecords){
            event.datasetRecords.SampleKey1.newValue = 'ModifyValue1';
            event.datasetRecords.SampleKey1.op = 'replace';
        }

        //Remove a key
        if('SampleKey2' in event.datasetRecords){
            event.datasetRecords.SampleKey2.op = 'remove';
        }

        //Add a key
        if(!('SampleKey3' in event.datasetRecords)){
            event.datasetRecords.SampleKey3={'newValue':'ModifyValue3', 'op' : 'replace'};
        }

    }
    context.done(null, event);
};
```