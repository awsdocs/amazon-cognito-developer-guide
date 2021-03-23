# Tagging Amazon Cognito Resources<a name="tagging"></a>

A *tag* is a metadata label that you assign or that AWS assigns to an AWS resource\. Each tag consists of a *key* and a *value*\. For tags that you assign, you define the key and value\. For example, you might define the key as `stage` and the value for one resource as `test`\.

Tags help you do the following:
+ Identify and organize your AWS resources\. Many AWS services support tagging, so you can assign the same tag to resources from different services to indicate that the resources are related\. For example, you could assign the same tag to an Amazon Cognito user pool that you assign to an Amazon DynamoDB table\.
+ Track your AWS costs\. You activate these tags on the AWS Billing and Cost Management dashboard\. AWS uses the tags to categorize your costs and deliver a monthly cost allocation report to you\. For more information, see [Use Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) in the *AWS Billing and Cost Management User Guide*\.
+ Control access to your resources based on the tags that are assigned to them\. You control access by specifying tag keys and values in the conditions for an AWS Identity and Access Management \(IAM\) policy\. For example, you could allow an IAM user to update a user pool, but only if the user pool has an `owner` tag with a value of that user's name\. For more information, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *IAM User Guide*\.

You can use the AWS CLI or the Amazon Cognito API to add, edit, or delete tags for user pools and identity pools\. For user pools specifically, you can also manage tags by using the Amazon Cognito console\.

For tips on using tags, see the [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/) post on the *AWS Answers* blog\. 

The following sections provide more information about tags for Amazon Cognito\.

## Supported Resources in Amazon Cognito<a name="tagging-supported-resources"></a>

The following resources in Amazon Cognito support tagging: 
+ User pools
+ Identity pools

## Tag Restrictions<a name="tagging-restrictions"></a>

The following basic restrictions apply to tags on Amazon Cognito resources:
+ Maximum number of tags that you can assign to a resource – 50 
+ Maximum key length – 128 Unicode characters 
+ Maximum value length – 256 Unicode characters 
+ Valid characters for key and value – a\-z, A\-Z, 0\-9, space, and the following characters: \_ \. : / = \+ \- and @
+ Keys and values are case sensitive
+ Don't use `aws:` as a prefix for keys; it's reserved for AWS use

## Managing Tags by Using the Amazon Cognito Console<a name="tagging-console"></a>

You can use the Amazon Cognito console to manage the tags that are assigned to your user pools\.

For identity pools, the console does not include tagging features, so you must manage tags programmatically\. For example, you can use the AWS CLI\.

**To add tags to a user pool**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito](https://console.aws.amazon.com/cognito)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to add tags to\.

1. In the navigation menu on the left, choose **Tags**\.

1. Unless your user pool already has tags, choose **Add tag** to add the first one\.

1. Specify values for **Tag Key** and **Tag Value**\.

1. For each additional tag that you want to add, choose **Add another tag**\.

1. When you are finished adding tags, choose **Save changes\.**

On the **Tags** page, you can also edit the keys and values of any existing tags\. To remove a tag, choose the *x* in the top\-right corner of the tag\.

## AWS CLI Examples<a name="tagging-cli-examples"></a>

The AWS CLI provides commands that you can use to manage the tags that you assign to your Amazon Cognito user pools and identity pools\. 

### Assigning tags<a name="tagging-cli-examples-assigning"></a>

Use the following commands to assign tags to your existing user pools and identity pools\.

**Example `tag-resource` command for user pools**  
Assign tags to a user pool by using [https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/tag-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/tag-resource.html) within the `cognito-idp` set of commands:  

```
$ aws cognito-idp tag-resource \
> --resource-arn user-pool-arn \
> --tags Stage=Test
```
This command includes the following parameters:  
+ `resource-arn` – The Amazon Resource Name \(ARN\) of the user pool that you are applying tags to\. To look up the ARN, choose the user pool in the Amazon Cognito console, and view the **Pool ARN** value on the **General settings** tab\.
+ `tags` – The key\-value pairs of the tags\.
To assign multiple tags at once, specify them in a comma\-separated list:  

```
$ aws cognito-idp tag-resource \
> --resource-arn user-pool-arn \
> --tags Stage=Test,CostCenter=80432,Owner=SysEng
```

**Example `tag-resource` command for identity pools**  
Assign tags to an identity pool by using [https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/tag-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/tag-resource.html) within the `cognito-identity` set of commands:  

```
$ aws cognito-identity tag-resource \
> --resource-arn identity-pool-arn \
> --tags Stage=Test
```
This command includes the following parameters:  
+ `resource-arn` – The Amazon Resource Name \(ARN\) of the identity pool that you are applying tags to\. To look up the ARN, choose the identity pool in the Amazon Cognito console, and choose **Edit identity pool**\. Then, at **Identity pool ID**, choose **Show ARN**\.
+ `tags` – The key\-value pairs of the tags\.
To assign multiple tags at once, specify them in a comma\-separated list:  

```
$ aws cognito-identity tag-resource \
> --resource-arn identity-pool-arn \
> --tags Stage=Test,CostCenter=80432,Owner=SysEng
```

### Viewing tags<a name="tagging-cli-examples-viewing"></a>

Use the following commands to view the tags that you have assigned to your user pools and identity pools\.

**Example `list-tags-for-resource` command for user pools**  
View the tags that are assigned to a user pool by using [https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/list-tags-for-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/list-tags-for-resource.html) within the `cognito-idp` set of commands:  

```
$ aws cognito-idp list-tags-for-resource --resource-arn user-pool-arn
```

**Example `list-tags-for-resource` command for identity pools**  
View the tags that are assigned to an identity pool by using [https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/list-tags-for-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/list-tags-for-resource.html) within the `cognito-identity` set of commands:  

```
$ aws cognito-identity list-tags-for-resource --resource-arn identity-pool-arn
```

### Removing tags<a name="tagging-cli-examples-removing"></a>

Use the following commands to remove tags from your user pools and identity pools\.

**Example `untag-resource` command for user pools**  
Remove tags from a user pool by using [https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/untag-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/untag-resource.html) within the `cognito-idp` set of commands:  

```
$ aws cognito-idp untag-resource \
> --resource-arn user-pool-arn \
> --tag-keys Stage CostCenter Owner
```
For the `--tag-keys` parameter, specify one or more tag keys, and do not include the tag values\.

**Example `untag-resource` command for identity pools**  
Remove tags from an identity pool by using [https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/untag-resource.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/untag-resource.html) within the `cognito-identity` set of commands:  

```
$ aws cognito-identity untag-resource \
> --resource-arn identity-pool-arn \
> --tag-keys Stage CostCenter Owner
```
For the `--tag-keys` parameter, specify one or more tag keys, and do not include the tag values\.

**Important**  
After you delete a user or identity pool, tags related to the deleted pool can still appear in the console or API calls up to thirty days after the deletion\.

### Applying tags when you create resources<a name="tagging-cli-examples-applying"></a>

Use the following commands to assign tags at the moment you create a user pool or identity pool\.

**Example `create-user-pool` command with tags**  
When you create a user pool by using the [https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/create-user-pool.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/create-user-pool.html) command, you can specify tags with the `--user-pool-tags` parameter:  

```
$ aws cognito-idp create-user-pool \
> --pool-name user-pool-name \
> --user-pool-tags Stage=Test,CostCenter=80432,Owner=SysEng
```

**Example `create-identity-pool` command with tags**  
When you create an identity pool by using the [https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/create-identity-pool.html](https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/create-identity-pool.html) command, you can specify tags with the `--identity-pool-tags` parameter:  

```
$ aws cognito-identity create-identity-pool \
> --identity-pool-name identity-pool-name \
> --allow-unauthenticated-identities \
> --identity-pool-tags Stage=Test,CostCenter=80432,Owner=SysEng
```

## Managing Tags by Using the Amazon Cognito API<a name="tagging-api"></a>

You can use the following actions in the Amazon Cognito API to manage the tags for your user pools and identity pools\.

### API Actions for User Pool Tags<a name="tagging-api-user-pools"></a>

Use the following API actions to assign, view, and remove tags for user pools\.
+ [TagResource](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_TagResource.html)
+ [ListTagsForResource](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListTagsForResource.html)
+ [UntagResource](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UntagResource.html)
+ [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html)

### API Actions for Identity Pool Tags<a name="tagging-api-identity-pools"></a>

Use the following API actions to assign, view, and remove tags for identity pools\.
+ [TagResource](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_TagResource.html)
+ [ListTagsForResource](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_ListTagsForResource.html)
+ [UntagResource](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_UntagResource.html)
+ [CreateIdentityPool](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_CreateIdentityPool.html)