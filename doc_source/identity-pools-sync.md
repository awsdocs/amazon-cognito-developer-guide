# Using Amazon Cognito Sync with identity pools<a name="identity-pools-sync"></a>

 Amazon Cognito Sync is an AWS service and client library that makes it possible to sync application\-related user data across devices\. Amazon Cognito Sync can synchronize user profile data across mobile devices and the web without using your own backend\. The client libraries cache data locally so that your app can read and write data regardless of device connectivity status\. When the device is online, you can synchronize data\. If you set up push sync, you can notify other devices immediately that an update is available\. 

## Managing datasets<a name="managing-datasets-in-the-amazon-cognito-console"></a>

If you have implemented Amazon Cognito Sync functionality in your application, the Amazon Cognito identity pools console enables you to manually create and delete datasets and records for individual identities\. Any change you make to an identity's dataset or records in the Amazon Cognito identity pools console isn't saved until you select **Synchronize** in the console\. The change isn't visible to the end user until the identity calls **Synchronize**\. The data being synchronized from other devices for individual identities is visible once you refresh the list datasets page for a particular identity\.

### Create a dataset for an identity<a name="create-a-dataset-for-an-identity"></a>

Choose **Manage identity pools** from the [Amazon Cognito console ](https://console.aws.amazon.com/cognito/home):

1.  Select the name of the identity pool that contains the identity for which you want to create a dataset\. The **Dashboard page** for your identity pool appears\. 

1.  In the left\-hand navigation on the Dashboard page, select **Identity browser**\. The **Identities** page appears\. 

1.  On the **Identities** page, enter the identity ID for which you want to create a dataset, and then select **Search**\. 

1.  On the **Identity details** page for that identity, select the **Create dataset** button, enter a dataset name, and then select **Create and edit dataset**\. 

1.  On the **Current dataset** page, select **Create record** to create a record to store in that dataset\. 

1.  Enter a key for that dataset, the valid JSON value or values to store, and then select **Format as JSON** to format the value you entered for readability, and to confirm that it is well\-formed JSON\. When finished, select **Save Changes**\. 

1.  Select **Synchronize** to synchronize the dataset\. Your changes will not be saved until you select **Synchronize** and will not be visible to the user until the identity calls **Synchronize**\. To discard unsynchronized changes, select the change you wish to discard, and then select **Discard changes**\. 

### Delete a dataset associated with an identity<a name="delete-a-dataset-associated-with-an-identity"></a>

Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home):

1.  Select the name of the identity pool that contains the identity for which you want to delete a dataset\. The **Dashboard page** for your identity pool appears\. 

1.  In the left\-hand navigation on the Dashboard page, select **Identity browser**\. The **Identities** page appears\. 

1.  On the **Identities** page, enter the identity ID containing the dataset which you want to delete, and then select **Search**\. 

1.  On the **Identity details** page, select the check box next to the dataset or datasets that you want to delete, select **Delete selected**, and then select **Delete**\. 

## Bulk publish data<a name="bulk-publish-data"></a>

 Bulk publish can be used to export data already stored in your Amazon Cognito Sync store to a Amazon Kinesis stream\. For instructions on how to bulk publish all of your streams, see [Amazon Cognito Streams](cognito-streams.md)\. 

## Enable push synchronization<a name="enable-push-synchronization"></a>

 Amazon Cognito automatically tracks the association between identity and devices\. Using the Push Sync feature, you can ensure that every instance of a given identity is notified when identity data changes\. Push Sync ensures that, whenever the sync store data changes for a particular identity, all devices associated with that identity receive a silent push notification informing them of the change\. 

 You can enable Push Sync via the Amazon Cognito console\. Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home): 

1.  Select the name of the identity pool for which you want to enable Push Sync\. The **Dashboard page** for your identity pool appears\. 

1.  In the top\-right corner of the Dashboard page, select **Edit identity pool**\. The **Edit identity pool** page appears\. 

1.  Scroll down and select **Push synchronization** to expand it\. 

1.  In the **Service role** dropdown list, select the IAM role that grants Amazon Cognito permission to send an Amazon Simple Notification Service \(Amazon SNS\) notification\. Select **Create role** to create or modify the roles associated with your identity pool in the [AWS IAM console](https://console.aws.amazon.com/iam/home)\. 

1.  Select a platform application, and then select **Save Changes**\. 

## Set up Amazon Cognito Streams<a name="set-up-cognito-streams"></a>

 Amazon Cognito Streams gives developers control and insight into their data stored in Amazon Cognito Sync\. Developers can now configure a Kinesis stream to receive events as data\. Amazon Cognito can push each dataset change to a Kinesis stream you own in real time\. For instructions on how to set up Amazon Cognito Streams in the Amazon Cognito console, see [Amazon Cognito Streams](cognito-streams.md)\. 

## Set up Amazon Cognito Events<a name="set-up-cognito-events"></a>

 Amazon Cognito Events allows you to execute an AWS Lambda function in response to important events in Amazon Cognito Sync\. Amazon Cognito Sync raises the Sync Trigger event when a dataset is synchronized\. You can use the Sync Trigger event to take an action when a user updates data\. For instructions on setting up Amazon Cognito Events from the console, see [Amazon Cognito Events](cognito-events.md)\. 

 To learn more about AWS Lambda, see [AWS Lambda](https://aws.amazon.com/lambda/)\. 