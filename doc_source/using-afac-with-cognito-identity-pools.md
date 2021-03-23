# Using attributes for access control with Amazon Cognito identity pools<a name="using-afac-with-cognito-identity-pools"></a>

Before you can use attributes for access control, ensure that you meet the following prerequisites:
+ [An AWS account](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html#aws-sign-up-identity-pools)
+ [User pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)
+ [Identity pool](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html#create-identity-pools)
+ [The mobile or JavaScript SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html##install-the-mobile-or-javascript-sdk)
+ [Integrated identity providers](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html##integrate-the-identity-providers)
+ [Credentials](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html#get-credentials)

To use attributes for access control you have to configure **Tag Key for Principal** and **Attribute name**\. In **Tag Key for Principal** the value is used to match the **PrincipalTag** condition in the permissions policies\. The value in **Attribute name** is the name of the attribute whose value will be evaluated in the policy\.

**To use attributes for access control with identity pools**

1. Open the Amazon Cognito console\.

1. Choose **Manage Identity Pools**\.

1. On the dashboard, choose the name of the identity pool that you want to use attributes for access control on\.

1. Choose **Edit identity pool**\.

1. Expand the **Authentication providers** section\. 

1. In the **Authentication providers** section, choose the provider tab you want to use\.

1. In **Attributes for access control**, choose either **Default attribute mappings** or **Custom attribute mappings**\. Default mappings are different for each provider\. For more information, see [Default provider mappings](provider-mappings.md) for attributes for access control\.

1. If you choose **Custom attribute mappings**, complete the following steps\.

   1. In **Tag Key for Principal**, enter your custom text\. There is a maximum length of 128 characters\. 

   1. In **Attribute name**, enter the attribute names from your providers tokens or SAML assertions\. You can get your attribute names for your IdPs from your providers developer guide\. Attribute names have a maximum of 256 characters\. In addition, the aggregated character limit for all attributes is 460 bytes\. 

   1. \(Optional\) **Add another provider**\. You can add multiple providers for Amazon Cognito user pools, OIDC, and SAML providers in the console\. For example, you can add two Amazon Cognito user pools as two separate identity providers\. Amazon Cognito treats each tab as different IdPs\. You can configure attributes for access control separately for each IdP\.

   1. To finish, use the IAM console to create a permissions policy that includes the default mappings or the custom text mappings that you provided in **Tag Key for Principal**\. For a tutorial on creating a permissions policy in IAM, see [IAM tutorial: Define permissions to access AWS resources based on tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_attribute-based-access-control.html) in the *IAM User Guide*\.