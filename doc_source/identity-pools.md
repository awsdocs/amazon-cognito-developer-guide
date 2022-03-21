# Using identity pools \(federated identities\)<a name="identity-pools"></a>

Amazon Cognito identity pools provide temporary AWS credentials for users who are guests \(unauthenticated\) and for users who have been authenticated and received a token\. An identity pool is a store of user identity data specific to your account\.

**To create a new identity pool in the console**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), choose **Manage identity pools**, and then choose **Create new identity pool**\.

1. Type a name for your identity pool\.

1. To enable unauthenticated identities, select **Enable access to unauthenticated identities** from the **Unauthenticated identities** collapsible section\.

1. If desired, configure an authentication provider in the **Authentication providers** section\.

1. Choose **Create Pool**\.
**Note**  
At least one identity is required for a valid identity pool\.

1. You will be prompted for access to your AWS resources\.

   Choose **Allow** to create the two default roles associated with your identity pool—one for unauthenticated users, and one for authenticated users\. These default roles provide your identity pool access to Amazon Cognito Sync\. You can modify the roles associated with your identity pool in the IAM console\. For additional instructions on working with the Amazon Cognito console, see [Using the Amazon Cognito console](cognito-console.md)\.

## User IAM roles<a name="user-iam-roles"></a>

An IAM role defines the permissions for your users to access AWS resources, like [Amazon Cognito Sync](cognito-sync.md)\. Users of your application will assume the roles you create\. You can specify different roles for authenticated and unauthenticated users\. To learn more about IAM roles, see [IAM roles](iam-roles.md)\.

## Authenticated and unauthenticated identities<a name="authenticated-and-unauthenticated-identities"></a>

Amazon Cognito identity pools support both authenticated and unauthenticated identities\. Authenticated identities belong to users who are authenticated by any supported identity provider\. Unauthenticated identities typically belong to guest users\.
+ To configure authenticated identities with a public login provider, see [Identity pools \(federated identities\) external identity providers](external-identity-providers.md)\.
+ To configure your own backend authentication process, see [Developer authenticated identities \(identity pools\)](developer-authenticated-identities.md)\.

## Enable or disable unauthenticated identities<a name="enable-or-disable-unauthenticated-identities"></a>

 Amazon Cognito identity pools can support unauthenticated identities by providing a unique identifier and AWS credentials for users who do not authenticate with an identity provider\. If your application allows users who do not log in, you can enable access for unauthenticated identities\. To learn more, see [Getting started with Amazon Cognito identity pools \(federated identities\)](getting-started-with-identity-pools.md)\.

Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home):

1. Select the name of the identity pool for which you want to enable or disable unauthenticated identities\. The **Dashboard page** for your identity pool appears\.

1. In the top\-right corner of the Dashboard page, select **Edit identity pool**\. The **Edit identity pool** page appears\.

1. Scroll down and select **Unauthenticated identities** to expand it\.

1. Select the check box to enable or disable access to unauthenticated identities\.

1. Select **Save Changes**\.

## Change the role associated with an identity type<a name="change-the-role-associated-with-an-identity-type"></a>

Every identity in your identity pool is either authenticated or unauthenticated\. Authenticated identities belong to users who are authenticated by a public login provider \(Amazon Cognito user pools, Login with Amazon, Sign in with Apple, Facebook, Google, SAML, or any OpenID Connect Providers\) or a developer provider \(your own backend authentication process\)\. Unauthenticated identities typically belong to guest users\.

For each identity type, there is an assigned role\. This role has a policy attached to it which dictates which AWS services that role can access\. When Amazon Cognito receives a request, the service determines the identity type, determines the role assigned to that identity type, and uses the policy attached to that role to respond\. By modifying a policy or assigning a different role to an identity type, you can control which AWS services an identity type can access\. To view or modify the policies associated with the roles in your identity pool, see the [AWS IAM Console](https://console.aws.amazon.com/iam/home)\.

 You can change which role is associated with an identity type using the Amazon Cognito identity pool \(federated identities\) console\. Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home):

1. Select the name of the identity pool for which you want to modify roles\. The **Dashboard page** for your identity pool appears\.

1. In the top\-right corner of the Dashboard page, select **Edit identity pool**\. The **Edit identity pool** page appears\.

1. Use the dropdown lists next to **Unauthenticated role** and **Authenticated role** to change roles\. Select **Create new role** to create or modify the roles associated with each identity type in the [AWS IAM console](https://console.aws.amazon.com/iam/home)\. For more information, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)\.

## Enable or edit authentication providers<a name="enable-or-edit-authentication-providers"></a>

If you allow your users to authenticate using public identity providers \(for example, Amazon Cognito user pools, Login with Amazon, Sign in with Apple, Facebook, or Google\), you can specify your application identifiers in the Amazon Cognito identity pools \(federated identities\) console\. This associates the application ID \(provided by the public login provider\) with your identity pool\.

You can also configure authentication rules for each provider from this page\. Each provider allows up to 25 rules\. The rules are applied in the order you save for each provider\. For more information, see [Role\-based access control](role-based-access-control.md)\.

**Warning**  
Changing the application ID to which an identity pool is linked disables existing users from authenticating with that identity pool\. Learn more about [Identity pools \(federated identities\) external identity providers](external-identity-providers.md)\.

Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home): 

1. Select the name of the identity pool for which you want to enable the external provider\. The **Dashboard page** for your identity pool appears\.

1. In the top\-right corner of the Dashboard page, select **Edit identity pool**\. The **Edit identity pool** page appears\.

1. Scroll down and select **Authentication providers** to expand it\.

1. Select the tab for the appropriate provider and enter the required information associated with that authentication provider\.

## Delete an identity pool<a name="delete-an-identity-pool"></a>

Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home): 

1. Select the name of the identity pool that you want to delete\. The **Dashboard page** for your identity pool appears\.

1. In the top\-right corner of the Dashboard page, select **Edit identity pool**\. The **Edit identity pool** page appears\.

1. Scroll down and select **Delete identity pool** to expand it\.

1. Select **Delete identity pool**\.

1. Select **Delete pool**\.

**Warning**  
When you select the Delete button, you will permanently delete your identity pool and all the user data it contains\. Deleting an identity pool will cause applications and other services utilizing the identity pool to stop working\.

## Delete an identity from an identity pool<a name="delete-an-identity-from-an-identity-pool"></a>

Choose **Manage identity pools** from the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home):

1. Select the name of the identity pool that contains the identity that you want to delete\. The **Dashboard page** for your identity pool appears\.

1. In the left\-hand navigation on the Dashboard page, select **Identity browser**\. The **Identities** page appears\.

1. On the **Identities** page, enter the identity ID that you want to delete, and then select **Search**\.

1. On the **Identity details** page, select the **Delete identity** button, and then select **Delete**\.