# Managing Datasets<a name="managing-datasets-in-the-amazon-cognito-console"></a>

 If you have implemented Amazon Cognito Sync functionality in your application, the Amazon Cognito Identity Pools console enables you to manually create and delete datasets and records for individual identities\. Any change you make to an identity's dataset or records in the Amazon Cognito Identity Pools console will not be saved until you click Synchronize in the console and will not be visible to the end user until the identity calls synchronize\. The data being synchronized from other devices for individual identities will be visible once you refresh the list datasets page for a particular identity\.

## Create a Dataset for an Identity<a name="create-a-dataset-for-an-identity"></a>

Choose **Manage Identity Pools** from the Amazon Cognito Identity Pools [Amazon Cognito console ](https://console.aws.amazon.com/cognito/home):

1.  Click the name of the identity pool that contains the identity for which you want to create a dataset\. The **Dashboard page** for your identity pool appears\. 

1.  In the left\-hand navigation on the Dashboard page, click **Identity browser**\. The **Identities** page appears\. 

1.  On the **Identities** page, enter the identity ID for which you want to create a dataset, and then click **Search**\. 

1.  On the **Identity details** page for that identity, click the **Create dataset** button, enter a dataset name, and then click **Create and edit dataset**\. 

1.  On the **Current dataset** page, click **Create record** to create a record to store in that dataset\. 

1.  Enter a key for that dataset, the valid JSON value or values to store, and then click **Format as JSON** to prettify the value you entered and to confirm that it is well\-formed JSON\. When finished, click **Save Changes**\. 

1.  Click **Synchronize** to synchronize the dataset\. Your changes will not be saved until you click Synchronize and will not be visible to the user until the identity calls synchronize\. To discard unsynchronized changes, select the change you wish to discard, and then click **Discard changes**\. 

## Delete a Dataset Associated with an Identity<a name="delete-a-dataset-associated-with-an-identity"></a>

Choose **Manage Identity Pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home):

1.  Click the name of the identity pool that contains the identity for which you want to delete a dataset\. The **Dashboard page** for your identity pool appears\. 

1.  In the left\-hand navigation on the Dashboard page, click **Identity browser**\. The **Identities** page appears\. 

1.  On the **Identities** page, enter the identity ID containing the dataset which you want to delete, and then click **Search**\. 

1.  On the **Identity details** page, select the checkbox next to the dataset or datasets that you want to delete, click **Delete selected**, and then click **Delete**\. 