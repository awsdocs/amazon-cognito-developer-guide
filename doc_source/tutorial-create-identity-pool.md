# Tutorial: Creating an Identity Pool<a name="tutorial-create-identity-pool"></a>

With an identity pool, your users can obtain temporary AWS credentials to access AWS services, such as Amazon S3 and DynamoDB\.

**To create an identity pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You may be prompted for your AWS credentials\.

1. Choose **Manage Identity Pools**

1.  Choose **Create new identity pool**\.

1. Enter a name for your identity pool\.

1. To enable unauthenticated identities select **Enable access to unauthenticated identities** from the **Unauthenticated identities** collapsible section\.

1. Choose **Create Pool**\.

1. You will be prompted for access to your AWS resources\.

   Choose **Allow** to create the two default roles associated with your identity pool–one for unauthenticated users and one for authenticated users\. These default roles provide your identity pool access to Amazon Cognito Sync\. You can modify the roles associated with your identity pool in the IAM console\.

1. Make a note of your identity pool Id number\. You will use it to set up policies allowing your app users to access other AWS services such as Amazon Simple Storage Service or DynamoDB

## Related Resources<a name="tutorial-related-resources-2"></a>

For more information on identity pools, see [Amazon Cognito Identity Pools \(Federated Identities\)](cognito-identity.md)\.

For an S3 example using an identity pool see [Uploading Photos to Amazon S3 from a Browser](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/s3-example-photo-album.html)\.