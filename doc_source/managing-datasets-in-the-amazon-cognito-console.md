# Managing datasets<a name="managing-datasets-in-the-amazon-cognito-console"></a>

If you have implemented Amazon Cognito Sync functionality in your application, the Amazon Cognito identity pools console enables you to manually create and delete datasets and records for individual identities\. Any change you make to an identity's dataset or records in the Amazon Cognito identity pools console isn't saved until you select **Synchronize** in the console\. The change isn't visible to the end user until the identity calls **Synchronize**\. The data being synchronized from other devices for individual identities is visible once you refresh the list datasets page for a particular identity\.

## Create a dataset for an identity<a name="create-a-dataset-for-an-identity"></a>

Choose **Manage identity pools** from the [Amazon Cognito console ](https://console.aws.amazon.com/cognito/home):

1.  Select the name of the identity pool that contains the identity for which you want to create a dataset\. The **Dashboard page** for your identity pool appears\. 

1.  In the left\-hand navigation on the Dashboard page, select **Identity browser**\. The **Identities** page appears\. 

1.  On the **Identities** page, enter the identity ID for which you want to create a dataset, and then select **Search**\. 

1.  On the **Identity details** page for that identity, select the **Create dataset** button, enter a dataset name, and then select **Create and edit dataset**\. 

1.  On the **Current dataset** page, select **Create record** to create a record to store in that dataset\. 

1.  Enter a key for that dataset, the valid JSON value or values to store, and then select **Format as JSON** to format the value you entered for readability, and to confirm that it is well\-formed JSON\. When finished, select **Save Changes**\. 

1.  Select **Synchronize** to synchronize the dataset\. Your changes will not be saved until you select **Synchronize** and will not be visible to the user until the identity calls **Synchronize**\. To discard unsynchronized changes, select the change you wish to discard, and then select **Discard changes**\. 

## Delete a dataset associated with an identity<a name="delete-a-dataset-associated-with-an-identity"></a>

Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home):

1.  Select the name of the identity pool that contains the identity for which you want to delete a dataset\. The **Dashboard page** for your identity pool appears\. 

1.  In the left\-hand navigation on the Dashboard page, select **Identity browser**\. The **Identities** page appears\. 

1.  On the **Identities** page, enter the identity ID containing the dataset which you want to delete, and then select **Search**\. 

1.  On the **Identity details** page, select the check box next to the dataset or datasets that you want to delete, select **Delete selected**, and then select **Delete**\. 