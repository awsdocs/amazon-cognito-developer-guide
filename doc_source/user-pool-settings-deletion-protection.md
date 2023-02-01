# User pool deletion protection<a name="user-pool-settings-deletion-protection"></a>

To make it so that your administrators don't accidentally delete your user pool, activate deletion protection\. With deletion protection active, you must confirm that you want to delete your user pool before you delete it\. When you delete a user pool in the AWS Management Console, you can deactivate deletion protection at the same time\. When you accept the prompt to deactivate deletion protection and confirm your intention to delete, as shown in the following image, Amazon Cognito deletes your user pool\.

![\[A screenshot from the AWS Management Console showing a prompt to delete a user pool with an included prompt to also deactivate deletion protection.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[A screenshot from the AWS Management Console showing a prompt to delete a user pool with an included prompt to also deactivate deletion protection.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

When you want to delete a user pool with an Amazon Cognito API request, you must first change `DeletionProtection` to `Inactive` in an [UpdateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPool.html) request\. If you don't deactivate deletion protection, Amazon Cognito returns an `InvalidParameterException` error\. After you deactivate deletion protection, you can delete the user pool in a [DeleteUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DeleteUserPool.html) request\.

Amazon Cognito activates **Deletion protection** by default when you create a new user pool in the AWS Management Console\. When you create a user pool with the `CreateUserPool` API, deletion protection is inactive by default\. To use this feature in user pools that you create with the AWS CLI or an AWS SDK, set the `DeletionProtection` parameter to `True`\.

You can activate or deactivate deletion protection status in the **Deletion protection** container in the **User pool settings** tab in the Amazon Cognito console\.

# To configure deletion protection

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **User pool settings** tab\. Locate **Deletion Protection** and select **Activate** or **Deactivate**\.

1. Confirm your choice in the next dialogue\.