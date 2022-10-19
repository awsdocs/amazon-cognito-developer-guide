# Tutorial: Cleaning up your AWS resources<a name="tutorial-cleanup-tutorial"></a>

**Important**  
Currently, you must configure Amazon Cognito identity pools in the original console, even if you have migrated to the new console for Amazon Cognito user pools\. From the new console, choose **Federated identities** to navigate to the identity pools console\.

**To delete an identity pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage Identity Pools**\.

1. Choose the name of the identity pool that you want to delete\. The **Dashboard page** for your identity pool appears\. 

1. In the top\-right corner of the Dashboard page, choose **Edit identity pool**\. The **Edit identity pool** page appears\. 

1. Scroll down and choose **Delete identity pool** to expand it\. 

1. Choose **Delete identity pool**\. 

1. Choose **Delete pool**\. 

------
#### [ Original console ]

**To delete a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose the user pool you created in the previous step\.

1. On the **Domain name** page under **App integration**, select **Delete domain**\.

1. Choose **Delete domain** when prompted to confirm\.

1. Go to the **General Settings** page\.

1. Select **Delete pool** in the upper right corner of the page\.

1. Enter **delete** and choose **Delete pool** when prompted to confirm\.

------
#### [ New console ]

**To delete a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. From the navigation pane, choose **User Pools**\.

1. If you have not created a domain for your user pool, select the radio button next to a user pool and select **Delete**\. Enter the name of the user pool to confirm, and stop here\.

1. If you have created a domain for your user pool, select the user pool\.

1. Navigate to the **App integration** tab for your user pool\.

1. Next to **Domain**, choose **Actions** and select **Delete Cognito domain** or **Delete custom domain**\.

1. Enter the domain name to confirm deletion\.

1. Return to the **User pools** list and select the radio button next to your user pool\. Select **Delete** and enter the name of the user pool to confirm\.

------