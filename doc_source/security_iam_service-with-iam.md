# How Amazon Cognito works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to Cognito, learn what IAM features are available to use with Cognito\.






**IAM features you can use with Amazon Cognito**  

| IAM feature | Cognito support | 
| --- | --- | 
|  [Identity\-based policies](#security_iam_service-with-iam-id-based-policies)  |    Yes  | 
|  [Resource\-based policies](#security_iam_service-with-iam-resource-based-policies)  |    No   | 
|  [Policy actions](#security_iam_service-with-iam-id-based-policies-actions)  |    Yes  | 
|  [Policy resources](#security_iam_service-with-iam-id-based-policies-resources)  |    Yes  | 
|  [Policy condition keys](#security_iam_service-with-iam-id-based-policies-conditionkeys)  |    Yes  | 
|  [ACLs](#security_iam_service-with-iam-acls)  |    No   | 
|  [ABAC \(tags in policies\)](#security_iam_service-with-iam-tags)  |    Partial  | 
|  [Temporary credentials](#security_iam_service-with-iam-roles-tempcreds)  |    Yes  | 
|  [Principal permissions](#security_iam_service-with-iam-principal-permissions)  |    No   | 
|  [Service roles](#security_iam_service-with-iam-roles-service)  |    Yes  | 
|  [Service\-linked roles](#security_iam_service-with-iam-roles-service-linked)  |    Yes  | 

To get a high\-level view of how Cognito and other AWS services work with most IAM features, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

## Identity\-based policies for Cognito<a name="security_iam_service-with-iam-id-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports identity\-based policies  |    Yes  | 

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as an IAM user, group of users, or role\. These policies control what actions users and roles can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. You can't specify the principal in an identity\-based policy because it applies to the user or role to which it is attached\. To learn about all of the elements that you can use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Identity\-based policy examples for Cognito<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of Cognito identity\-based policies, see [Identity\-based policy examples for Amazon Cognito](security_iam_id-based-policy-examples.md)\.

## Resource\-based policies within Cognito<a name="security_iam_service-with-iam-resource-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports resource\-based policies  |    No   | 

Resource\-based policies are JSON policy documents that you attach to a resource\. Examples of resource\-based policies are IAM *role trust policies* and Amazon S3 *bucket policies*\. In services that support resource\-based policies, service administrators can use them to control access to a specific resource\. For the resource where the policy is attached, the policy defines what actions a specified principal can perform on that resource and under what conditions\. You must [specify a principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) in a resource\-based policy\. Principals can include accounts, users, roles, federated users, or AWS services\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the principal in a resource\-based policy\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, an IAM administrator in the trusted account must also grant the principal entity \(user or role\) permission to access the resource\. They grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM roles differ from resource\-based policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)in the *IAM User Guide*\.

## Policy actions for Cognito<a name="security_iam_service-with-iam-id-based-policies-actions"></a>


|  |  | 
| --- |--- |
|  Supports policy actions  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.



To see a list of Cognito actions, see [Actions defined by Amazon Cognito](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncognitoidentity.html#amazoncognitoidentity-actions-as-permissions) in the *Service Authorization Reference*\.

Policy actions in Cognito use the following prefix before the action:

```
cognito-identity
```

To specify multiple actions in a single statement, separate them with commas\.

```
"Action": [
      "cognito-identity:action1",
      "cognito-identity:action2"
         ]
```

### Signed versus unsigned APIs<a name="security_amazon-cognito-signed-versus-unsigned-apis"></a>

When you sign Amazon Cognito API requests with AWS credentials, you can restrict them in an AWS Identity and Access Management \(IAM\) policy\. API requests that you must sign with AWS credentials include server\-side sign\-in with `AdminInitiateAuth`, and actions that create, view, or modify your Amazon Cognito resources like `UpdateUserPool`\. For more information about signed API requests, see [Signing AWS API requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)\.

Because Amazon Cognito is a consumer identity product for apps that you want to make available to the public, you have access to the following unsigned APIs\. Your app makes these API requests for your users and your prospective users\. Some APIs require no prior authorization, like `InitiateAuth` to start a new authentication session\. Some APIs use access tokens or session keys for authorization, like `VerifySoftwareToken` to complete MFA setup for a user that has an existing authenticated session\. An unsigned, authorized Amazon Cognito user pools API supports a `Session` or `AccessToken` parameter in the request syntax as displayed in the [Amazon Cognito API Reference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html)\. An unsigned Amazon Cognito Identity API supports an `IdentityId` parameter as displayed in the [Amazon Cognito Federated Identities API Reference](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/Welcome.html)\.

**Amazon Cognito federated identities**
+ `GetId`
+ `GetOpenIdToken`
+ `GetCredentialsForIdentity`
+ `UnlinkIdentity`

**Amazon Cognito your user pools**
+ `AssociateSoftwareToken`
+ `ChangePassword`
+ `ConfirmDevice`
+ `ConfirmForgotPassword`
+ `ConfirmSignUp`
+ `DeleteUser`
+ `DeleteUserAttributes`
+ `ForgetDevice`
+ `ForgotPassword`
+ `GetDevice`
+ `GetUser`
+ `GetUserAttributeVerificationCode`
+ `GlobalSignOut`
+ `InitiateAuth`
+ `ListDevices`
+ `ResendConfirmationCode`
+ `RespondToAuthChallenge`
+ `RevokeToken`
+ `SetUserMFAPreference`
+ `SetUserSettings`
+ `SignUp`
+ `UpdateAuthEventFeedback`
+ `UpdateDeviceStatus`
+ `UpdateUserAttributes`
+ `VerifySoftwareToken`
+ `VerifyUserAttribute`





To view examples of Cognito identity\-based policies, see [Identity\-based policy examples for Amazon Cognito](security_iam_id-based-policy-examples.md)\.

## Policy resources for Cognito<a name="security_iam_service-with-iam-id-based-policies-resources"></a>


|  |  | 
| --- |--- |
|  Supports policy resources  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```

### Amazon resource names \(ARNs\)<a name="security_amazon-cognito-amazon-resource-names"></a>

**ARNs for Amazon Cognito federated identities**

In Amazon Cognito identity pools \(federated identities\), it is possible to restrict an IAM user's access to a specific identity pool, using the Amazon Resource Name \(ARN\) format, as in the following example\. For more information about ARNs, see [IAM identifiers](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html)\.

```
arn:aws:cognito-identity:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID
```

**ARNs for Amazon Cognito Sync**

In Amazon Cognito Sync, customers can also restrict access by the identity pool ID, identity ID, and dataset name\.

For APIs that operate on an identity pool, the identity pool ARN format is the same as for Amazon Cognito Federated Identities, except that the service name is `cognito-sync` instead of `cognito-identity`:

```
arn:aws:cognito-sync:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID
```

For APIs that operate on a single identity, such as `RegisterDevice`, you can refer to the individual identity by the following ARN format:

```
arn:aws:cognito-sync:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID/identity/IDENTITY_ID
```

For APIs that operate on datasets, such as `UpdateRecords` and `ListRecords`, you can refer to the individual dataset using the following ARN format:

```
arn:aws:cognito-sync:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID/identity/IDENTITY_ID/dataset/DATASET_NAME
```

**ARNs for Amazon Cognito user pools**

For Amazon Cognito Your User Pools, it is possible to restrict a user's access to a specific user pool, using the following ARN format:

```
arn:aws:cognito-idp:REGION:ACCOUNT_ID:userpool/USER_POOL_ID
```

To see a list of Cognito resource types and their ARNs, see [Resources defined by Amazon Cognito](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncognitoidentity.html#amazoncognitoidentity-resources-for-iam-policies) in the *Service Authorization Reference*\. To learn with which actions you can specify the ARN of each resource, see [Actions defined by Amazon Cognito](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncognitoidentity.html#amazoncognitoidentity-actions-as-permissions)\.





To view examples of Cognito identity\-based policies, see [Identity\-based policy examples for Amazon Cognito](security_iam_id-based-policy-examples.md)\.

## Policy condition keys for Cognito<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>


|  |  | 
| --- |--- |
|  Supports service\-specific policy condition keys  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

To see a list of Cognito condition keys, see [Condition keys for Amazon Cognito](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncognitoidentity.html#amazoncognitoidentity-policy-keys) in the *Service Authorization Reference*\. To learn with which actions and resources you can use a condition key, see [Actions defined by Amazon Cognito](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncognitoidentity.html#amazoncognitoidentity-actions-as-permissions)\.

To view examples of Cognito identity\-based policies, see [Identity\-based policy examples for Amazon Cognito](security_iam_id-based-policy-examples.md)\.

## Access control lists \(ACLs\) in Cognito<a name="security_iam_service-with-iam-acls"></a>


|  |  | 
| --- |--- |
|  Supports ACLs  |    No   | 

Access control lists \(ACLs\) control which principals \(account members, users, or roles\) have permissions to access a resource\. ACLs are similar to resource\-based policies, although they do not use the JSON policy document format\.

## Attribute\-based access control \(ABAC\) with Cognito<a name="security_iam_service-with-iam-tags"></a>


|  |  | 
| --- |--- |
|  Supports ABAC \(tags in policies\)  |    Partial  | 

Attribute\-based access control \(ABAC\) is an authorization strategy that defines permissions based on attributes\. In AWS, these attributes are called *tags*\. You can attach tags to IAM entities \(users or roles\) and to many AWS resources\. Tagging entities and resources is the first step of ABAC\. Then you design ABAC policies to allow operations when the principal's tag matches the tag on the resource that they are trying to access\.

ABAC is helpful in environments that are growing rapidly and helps with situations where policy management becomes cumbersome\.

To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `aws:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\.

If a service supports all three condition keys for every resource type, then the value is **Yes** for the service\. If a service supports all three condition keys for only some resource types, then the value is **Partial**\.

For more information about ABAC, see [What is ABAC?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_attribute-based-access-control.html) in the *IAM User Guide*\. To view a tutorial with steps for setting up ABAC, see [Use attribute\-based access control \(ABAC\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_attribute-based-access-control.html) in the *IAM User Guide*\.

## Using temporary credentials with Cognito<a name="security_iam_service-with-iam-roles-tempcreds"></a>


|  |  | 
| --- |--- |
|  Supports temporary credentials  |    Yes  | 

Some AWS services don't work when you sign in using temporary credentials\. For additional information, including which AWS services work with temporary credentials, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

You are using temporary credentials if you sign in to the AWS Management Console using any method except a user name and password\. For example, when you access AWS using your company's single sign\-on \(SSO\) link, that process automatically creates temporary credentials\. You also automatically create temporary credentials when you sign in to the console as a user and then switch roles\. For more information about switching roles, see [Switching to a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html) in the *IAM User Guide*\.

You can manually create temporary credentials using the AWS CLI or AWS API\. You can then use those temporary credentials to access AWS\. AWS recommends that you dynamically generate temporary credentials instead of using long\-term access keys\. For more information, see [Temporary security credentials in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)\.

## Cross\-service principal permissions for Cognito<a name="security_iam_service-with-iam-principal-permissions"></a>


|  |  | 
| --- |--- |
|  Supports principal permissions  |    No   | 

  When you use an IAM user or role to perform actions in AWS, you are considered a principal\. Policies grant permissions to a principal\. When you use some services, you might perform an action that then triggers another action in a different service\. In this case, you must have permissions to perform both actions\. To see whether an action requires additional dependent actions in a policy, see [Actions, resources, and condition keys for Amazon Cognito](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncognitoidentity.html) in the *Service Authorization Reference*\. 

## Service roles for Cognito<a name="security_iam_service-with-iam-roles-service"></a>


|  |  | 
| --- |--- |
|  Supports service roles  |    Yes  | 

  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 

For details about Cognito service roles, see [Enable push synchronization](identity-pools-sync.md#enable-push-synchronization) and [Push sync](push-sync.md)\.

**Warning**  
Changing the permissions for a service role might break Cognito functionality\. Edit service roles only when Cognito provides guidance to do so\.

## Service\-linked roles for Cognito<a name="security_iam_service-with-iam-roles-service-linked"></a>


|  |  | 
| --- |--- |
|  Supports service\-linked roles  |    Yes  | 

  A service\-linked role is a type of service role that is linked to an AWS service\. The service can assume the role to perform an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view, but not edit the permissions for service\-linked roles\. 

For details about creating or managing Cognito service\-linked roles, see [Using service\-linked roles for Amazon Cognito](using-service-linked-roles.md)\.