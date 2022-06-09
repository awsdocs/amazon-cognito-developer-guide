# Using service\-linked roles for Amazon Cognito<a name="using-service-linked-roles"></a>

Amazon Cognito uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Amazon Cognito\. Service\-linked roles are predefined by Amazon Cognito and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Amazon Cognito easier because you don’t have to manually add the necessary permissions\. Amazon Cognito defines the permissions of its service\-linked roles, and unless defined otherwise, only Amazon Cognito can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Amazon Cognito resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Amazon Cognito<a name="slr-permissions"></a>

Amazon Cognito uses the following service\-linked roles:
+ ** AWSServiceRoleForAmazonCognitoIdpEmailService** – Allows Amazon Cognito user pools service to use your Amazon SES identities for sending email\.
+ ** AWSServiceRoleForAmazonCognitoIdp** – Allows Amazon Cognito user pools to publish events and configure endpoints for your Amazon Pinpoint projects\.

**AWSServiceRoleForAmazonCognitoIdpEmailService**

The `AWSServiceRoleForAmazonCognitoIdpEmailService` service\-linked role trusts the following services to assume the role:
+ `email.cognito-idp.amazonaws.com`

The role permissions policy allows Amazon Cognito to complete the following actions on the specified resources:

**Allowed Actions for AWSServiceRoleForAmazonCognitoIdpEmailService:**
+ Action: `ses:SendEmail` and `ses:SendRawEmail`
+ Resource: `*`

The policy denies Amazon Cognito the ability to complete the following actions on the specified resources:

**Denied Actions**
+ Action: `ses:List*`
+ Resource: `*`

With these permissions, Amazon Cognito can use your verified email addresses in Amazon SES only to email your users\. Amazon Cognito emails your users when they perform certain actions in the client app for a user pool, such as signing up or resetting a password\.

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

**AWSServiceRoleForAmazonCognitoIdp**

The AWSServiceRoleForAmazonCognitoIdp service\-linked role trusts the following services to assume the role:
+ `email.cognito-idp.amazonaws.com`

The role permissions policy allows Amazon Cognito to complete the following actions on the specified resources:

**Allowed Actions for AWSServiceRoleForAmazonCognitoIdp**
+ Action: `cognito-idp:Describe` 
+ Resource: `*`

With this permission, Amazon Cognito can call `Describe` Amazon Cognito API operations for you\.

**Note**  
When you integrate Amazon Cognito with Amazon Pinpoint using `createUserPoolClient` and `updateUserPoolClient`, resource permissions will be added to the SLR as an inline policy\. The inline policy will provide `mobiletargeting:UpdateEndpoint` and `mobiletargeting:PutEvents` permissions\. These permissions allow Amazon Cognito to publish events and configure endpoints for Pinpoint projects you integrate with Cognito\.

## Creating a service\-linked role for Amazon Cognito<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you configure a user pool to use your Amazon SES configuration to handle email delivery in the AWS Management Console, the AWS CLI, or the Amazon Cognito API, Amazon Cognito creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you configure a user pool to use your Amazon SES configuration to handle email delivery, Amazon Cognito creates the service\-linked role for you again\. 

Before Amazon Cognito can create this role, the IAM permissions that you use to set up your user pool must include the `iam:CreateServiceLinkedRole` action\. For more information about updating permissions in IAM, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

## Editing a service\-linked role for Amazon Cognito<a name="edit-slr"></a>

 You can't edit the AmazonCognitoIdp or AmazonCognitoIdpEmailService service\-linked roles in AWS Identity and Access Management\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Amazon Cognito<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. If you delete the role, you only retain entities that Amazon Cognito actively monitors or maintains\. Before you can delete AmazonCognitoIdp or AmazonCognitoIdpEmailService service\-linked roles, you must do one of the following for each user pool that uses the role:
+ Delete the user pool\.
+ Update the email settings in the user pool to use the default email functionality\. The default setting doesn't use the service\-linked role\.

Remember to perform the action in each AWS Region with a user pool that uses the role\.

**Note**  
If the Amazon Cognito service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete an Amazon Cognito user pool**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to delete\.

1. Choose **Delete pool**\.

1. In the **Delete user pool** window, type **delete**, and choose **Delete pool**\.

**To update an Amazon Cognito user pool to use the default email functionality**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to update\.

1. In the navigation menu on the left, choose **Message customizations**\.

1. Under **Do you want to send emails through your Amazon SES Configuration?**, choose **No \- Use Cognito \(Default\)**\.

1. When you finish setting your email account options, choose **Save changes**\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete AmazonCognitoIdp or AmazonCognitoIdpEmailService service\-linked roles\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for Amazon Cognito service\-linked roles<a name="slr-regions"></a>

Amazon Cognito supports service\-linked roles in all AWS Regions where the service is available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#cognito_identity_region)\.