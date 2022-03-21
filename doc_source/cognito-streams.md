# Amazon Cognito Streams<a name="cognito-streams"></a>

****  
If you're new to Amazon Cognito Sync, use [AWS AppSync](https://aws.amazon.com/appsync/)\. Like Amazon Cognito Sync, AWS AppSync is a service for synchronizing application data across devices\.  
It enables user data like app preferences or game state to be synchronized\. It also extends these capabilities by allowing multiple users to synchronize and collaborate in real time on shared data\.

Amazon Cognito Streams gives developers control and insight into their data stored in Amazon Cognito\. Developers can now configure a Kinesis stream to receive events as data is updated and synchronized\. Amazon Cognito can push each dataset change to a Kinesis stream you own in real time\.

Using Amazon Cognito Streams, you can move all of your Sync data to Kinesis, which can then be streamed to a data warehouse tool such as Amazon Redshift for further analysis\. To learn more about Kinesis, see [Getting Started Using Amazon Kinesis](https://docs.aws.amazon.com/kinesis/latest/dev/getting-started.html)\.

**Configuring streams**

You can set up Amazon Cognito Streams in the Amazon Cognito console\. To enable Amazon Cognito Streams in the Amazon Cognito console, you need to select the Kinesis stream to publish to and an IAM role that grants Amazon Cognito permission to put events in the selected stream\.

From the [console home page](https://console.aws.amazon.com/cognito/home):

1. Click the name of the identity pool for which you want to set up Amazon Cognito Streams\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, click **Manage Identity Pools**\. The Manage Federated Identities page appears\.

1. Scroll down and click **Cognito Streams** to expand it\.

1. In the **Stream name** dropdown menu, select the name of an existing Kinesis stream\. Alternatively, click **Create stream** to create one, entering a stream name and the number of shards\. To learn about shards and for help on estimating the number of shards needed for your stream, see the [Kinesis Developer Guide](https://docs.aws.amazon.com/kinesis/latest/dev/amazon-kinesis-streams.html)\.

1. In the **Publish role** dropdown menu, select the IAM role that grants Amazon Cognito permission to publish your stream\. Click **Create role** to create or modify the roles associated with your identity pool in the [AWS IAM Console](https://console.aws.amazon.com/iam/home)\.

1. In the **Stream status** dropdown menu, select **Enabled** to enable the stream updates\. Click **Save Changes**\.

After you've successfully configured Amazon Cognito streams, all subsequent updates to datasets in this identity pool will be sent to the stream\.

**Stream contents**

Each record sent to the stream represents a single synchronization\. Here is an example of a record sent to the stream:

```
{
    "identityPoolId": "Pool Id",
    "identityId": "Identity Id",
    "dataSetName": "Dataset Name",
    "operation": "(replace|remove)",
    "kinesisSyncRecords": [
        {
            "key": "Key",
            "value": "Value",
            "syncCount": 1,
            "lastModifiedDate": 1424801824343,
            "deviceLastModifiedDate": 1424801824343,
            "op": "(replace|remove)"
        },
        ...
    ],
    "lastModifiedDate": 1424801824343,
    "kinesisSyncRecordsURL": "S3Url",
    "payloadType": "(S3Url|Inline)",
    "syncCount": 1
}
```

For updates that are larger than the Kinesis maximum payload size of 1 MB, Amazon Cognito incudes a presigned Amazon S3 URL that contains the full contents of the update\.

After you have configured Amazon Cognito streams, if you delete the Kinesis stream or change the role trust permission so that Amazon Cognito Sync can no longer assume the role, you turn off the Amazon Cognito streams\. You must either recreate the Kinesis stream or fix the role, and then you must turn on the stream again\.

**Bulk publishing**

Once you have configured Amazon Cognito streams, you will be able to execute a bulk publish operation for the existing data in your identity pool\. After you initiate a bulk publish operation, either via the console or directly via the API, Amazon Cognito will start publishing this data to the same stream that is receiving your updates\.

Amazon Cognito does not guarantee uniqueness of data sent to the stream when using the bulk publish operation\. You may receive the same update both as an update as well as part of a bulk publish\. Keep this in mind when processing the records from your stream\.

To bulk publish all of your streams, follow steps 1\-6 under Configuring Streams and then click Start bulk publish\. You are limited to one ongoing bulk publish operation at any given time and to one successful bulk publish request every 24 hours\.