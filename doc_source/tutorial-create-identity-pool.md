# Tutorial: Creating an identity pool<a name="tutorial-create-identity-pool"></a>

**Important**  
Currently, you must configure Amazon Cognito identity pools in the original console, even if you have migrated to the new console for Amazon Cognito user pools\. From the new console, choose **Federated identities** to navigate to the identity pools console\.

With an identity pool, your users can obtain temporary AWS credentials to access AWS services, such as Amazon S3 and DynamoDB\.

**Note**  
In the new Amazon Cognito console experience, you can manage identity pools from the **Federated identities** link on the left navigation bar\.

**To create an identity pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/federated)\. If prompted, enter your AWS credentials\.

1. Choose **Manage Identity Pools**\.

1. Choose **Create new identity pool**\.

1. Enter a name for your identity pool\.

1. To enable unauthenticated identities, select **Enable access to unauthenticated identities** from the **Unauthenticated identities** collapsible section\.

1. Choose **Create Pool**\.

1. You will be prompted for access to your AWS resources\.

   Choose **Allow** to create the two default roles associated with your identity pool: one for unauthenticated users and one for authenticated users\. These default roles provide your identity pool access to Amazon Cognito Sync\. You can modify the roles associated with your identity pool in the IAM console\.

1. Make a note of your identity pool Id number\. You will use it to set up policies that will allow your app users to access other AWS services, such as Amazon Simple Storage Service or DynamoDB

## Related resources<a name="tutorial-related-resources-2"></a>

For more information on identity pools, see [Amazon Cognito identity pools \(federated identities\)](cognito-identity.md)\.

For an example of using an identity pool with Amazon S3, see [Uploading Photos to Amazon S3 from a Browser](https://docs.aws.amazon.com/sdk-for-javascript/latest/developer-guide/s3-example-photo-album.html)\.