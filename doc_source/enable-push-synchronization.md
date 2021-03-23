# Enable Push Synchronization<a name="enable-push-synchronization"></a>

 Amazon Cognito automatically tracks the association between identity and devices\. Using the push sync feature, you can ensure that every instance of a given identity is notified when identity data changes\. Push sync ensures that, whenever the sync store data changes for a particular identity, all devices associated with that identity receive a silent push notification informing them of the change\. 

 You can enable Push Sync via the Amazon Cognito console\. Choose **Manage Identity Pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home): 

1.  Click the name of the identity pool for which you want to enable Push Sync\. The **Dashboard page** for your identity pool appears\. 

1.  In the top\-right corner of the Dashboard page, click **Edit identity pool**\. The **Edit identity pool** page appears\. 

1.  Scroll down and click **Push synchronization** to expand it\. 

1.  In the **Service role** dropdown menu, select the IAM role that grants Amazon Cognito permission to send an SNS notification\. Click **Create role** to create or modify the roles associated with your identity pool in the [AWS IAM console](https://console.aws.amazon.com/iam/home)\. 

1.  Select a platform application, and then click **Save Changes**\. 