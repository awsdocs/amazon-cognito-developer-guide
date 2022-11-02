# AWS managed policies for Amazon Cognito<a name="security-iam-awsmanpol"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

A number of policies are available via the IAM Console that customers can use to grant access to Amazon Cognito:
+ `AmazonCognitoPowerUser` \- Permissions for accessing and managing all aspects of your identity pools and user pools\. To view the permissions for this policy, see [AmazonCognitoPowerUser](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonCognitoPowerUser)\.
+ `AmazonCognitoReadOnly` \- Permissions for read\-only access to your identity pools and user pools\. To view the permissions for this policy, see [AmazonCognitoReadOnly](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonCognitoReadOnly)\.
+ `AmazonCognitoDeveloperAuthenticatedIdentities` \- Permissions for your authentication system to integrate with Amazon Cognito\. To view the permissions for this policy, see [AmazonCognitoDeveloperAuthenticatedIdentities](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonCognitoDeveloperAuthenticatedIdentities)\.

These policies are maintained by the Amazon Cognito team, so even as new APIs are added your IAM users will continue to have the same level of access\.

**Note**  
Because creating a new identity pool also requires creating IAM roles, any IAM user you want to be able to create new identity pools with must have the admin policy applied as well\.













## Amazon Cognito updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Amazon Cognito since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon Cognito [Document history](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-document-history.html) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  `AmazonCognitoPowerUser` and `AmazonCognitoReadOnly` – Updates to existing policies  | Added new permissions to allow power users to view and manage associations of AWS WAF web ACLs to Amazon Cognito user pools\.Added new permissions to allow read\-only users to view associations of AWS WAF web ACLs to Amazon Cognito user pools\. | July 19, 2022 | 
| AmazonCognitoPowerUser – Update to an existing policy | Added a new permission to allow Amazon Cognito to call Amazon Simple Email Service PutIdentityPolicy and ListConfigurationSets operations\.This change allows Amazon Cognito user pools to update Amazon SES sending authorization policies and to apply Amazon SES configuration sets when you configure email sending in your user pool\. | November 17, 2021 | 
| AmazonCognitoPowerUser – Update to an existing policy |  Added a new permission to allow Amazon Cognito to call Amazon Simple Notification Service's `GetSMSSandboxAccountStatus` operation\. This change allows Amazon Cognito user pools to decide if you need to graduate out of the Amazon Simple Notification Service sandbox in order to send messages to all end users through user pools\.  | June 1, 2021 | 
|  Amazon Cognito started tracking changes  |  Amazon Cognito started tracking changes for its AWS managed policies\.  | March 1, 2021 | 