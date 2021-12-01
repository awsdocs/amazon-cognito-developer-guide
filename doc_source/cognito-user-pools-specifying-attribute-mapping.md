# Specifying identity provider attribute mappings for your user pool<a name="cognito-user-pools-specifying-attribute-mapping"></a>

You can use the AWS Management Console, or the AWS CLI or API, to specify attribute mappings for your user pool's identity providers\.

## Things to know about mappings<a name="cognito-user-pools-specifying-attribute-mapping-requirements"></a>

Before using mappings, review the following important details:
+ A mapping must be present for each user pool attribute that's required when a user signs in to your application\. For example, if your user pool requires an `email` attribute for sign\-in, map this attribute to its equivalent from the identity provider\.
+ By default, mapped email addresses are unverified\. You can't verify a mapped email address using a one\-time code\. Instead, map an attribute from your identity provider to get the verification status\. For example, Google and most OIDC providers include the `email_verified` attribute\.
+ For each mapped user pool attribute, the maximum value length \(2048 characters\) must be large enough for the value that Amazon Cognito obtains from the identity provider\. Otherwise, Amazon Cognito reports an error when users sign in to your application\. If you map a custom attribute to an identity provider token, set the length to 2048 characters\.
+ The `username` user pools attribute must be mapped only to specific attributes for the following identity providers:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-specifying-attribute-mapping.html)
+ Amazon Cognito must be able to update your mapped user pool attributes when users sign in to your application\. When a user signs in through an identity provider, Amazon Cognito updates the mapped attributes with the latest information from the identity provider\. Amazon Cognito updates each mapped attribute, even if its current value already matches the latest information\. If Amazon Cognito can't update the attribute, it reports an error\. To ensure that Amazon Cognito can update the attributes, check the following requirements:
  + The mapped user pool attributes must be *mutable*\. Mutable attributes can be updated by app clients that have write permissions for those attributes\. You can set user pool attributes as mutable when you define them in the Amazon Cognito console\. Or, if you create your user pool by using the [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) API operation, you can set the `Mutable` parameter for each of these attributes to `true`\.
  + In the app client settings for your application, the mapped attributes must be *writable*\. You can set which attributes are writable in the **App clients** page in the Amazon Cognito console\. Or, if you create the app client by using the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html) API operation, you can add these attributes to the `WriteAttributes` array\.
+ In the case of identity provider attributes containing multiple values, Amazon Cognito will URL form encode the values containing non\-alphanumeric characters \(excluding the ‘`.`’, ‘`-`’, ‘`*`’, and ‘`_`’ characters\)\. You must decode the values prior to use in your application\.

## Specifying identity provider attribute mappings for your user pool \(AWS Management Console\)<a name="cognito-user-pools-specifying-attribute-mapping-console"></a>

You can use the AWS Management Console to specify attribute mappings for your user pool's identity providers\.

**Note**  
Amazon Cognito will map incoming claims to user pool attributes only if the claims exist in the incoming token\. If a previously mapped claim no longer exists in the incoming token, it won't be deleted or changed\. If your application requires mapping of deleted claims, you can use the Pre\-Authentication Lambda trigger to delete the custom attribute during authentication and allow these attributes to re\-populate from the incoming token\.

------
#### [ Original console ]

**To specify a social identity provider attribute mapping**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials

1. In the navigation pane, choose **Manage your User Pools**, and choose the user pool you want to edit\.

1. Choose the **Attribute mapping** tab\.

1. Choose the **Facebook**, **Google**, **Amazon** or **Apple** tab\.

1. For each attribute you need to map, complete the following steps:

   1. Select the **Capture** check box\.

   1. For **User pool attribute**, in the drop\-down list, choose the user pool attribute you want to map to the social identity provider attribute\.

   1. If you need more attributes, choose **Add Facebook attribute** \(or **Add Google attribute** or **Add Amazon attribute** or **Add Apple attribute**\) and complete the following steps:

      1. In the **Facebook attribute**,**Google attribute**, **Amazon attribute**, or **Apple attribute** field, enter the name of the attribute to be mapped\.

      1. In the **User pool attribute** drop\-down list, choose the user pool attribute to map to the social identity provider attribute\.

   1. Choose **Save changes**\.

**To specify a SAML provider attribute mapping**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. In the navigation pane, choose **Manage your User Pools**, and choose the user pool you want to edit\.

1. Choose the **Attribute mapping** tab\.

1. Choose the **SAML** tab\.

1. Select the **Capture** box for all attributes for which you want to capture values\. If you clear the **Capture** box for an attribute and save your changes, that attribute's mapping is removed\.

1. Choose the identity provider from the drop\-down list\.

1. For each attribute you need to map, complete the following steps:

   1. Choose **Add SAML attribute**\.

   1. In the **SAML attribute** field, enter the name of the SAML attribute to be mapped\.

   1. In the **User pool attribute** field, choose the user pool attribute to map the SAML attribute to from the drop\-down list\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To specify a social identity provider attribute mapping**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. In the navigation pane, choose **User Pools**, and choose the user pool you want to edit\.

1. Choose the **Sign\-in experience** tab and locate **Federated sign\-in**\.

1. Choose **Add an identity provider**, or choose the **Facebook**, **Google**, **Amazon** or **Apple** identity provider you have configured\. Locate **Attribute mapping** and choose **Edit**\. 

   For more information about adding a social identity provider, see [Adding social identity providers to a user pool](cognito-user-pools-social-idp.md)\.

1. For each attribute you need to map, complete the following steps:

   1. Select an attribute from the **User pool attribute** column\. This is the attribute that is assigned to the user profile in your user pool\. Custom attributes are listed after standard attributes\.

   1. Select an attribute from the ***<provider>* attribute** column\. This will be the attribute passed from the provider directory\. Known attributes from the social provider are provided in a drop\-down list\.

   1. To map additional attributes between your IdP and Amazon Cognito, choose **Add another attribute**\.

1. Choose **Save changes**\.

**To specify a SAML provider attribute mapping**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. In the navigation pane, choose **User Pools**, and choose the user pool you want to edit\.

1. Choose the **Sign\-in experience** tab and locate **Federated sign\-in**\.

1. Choose **Add an identity provider**, or choose the SAML identity provider you have configured\. Locate **Attribute mapping**, and choose **Edit**\. For more information about adding a SAML identity provider, see [Adding SAML identity providers to a user pool](cognito-user-pools-saml-idp.md)\.

1. For each attribute you need to map, complete the following steps:

   1. Select an attribute from the **User pool attribute** column\. This is the attribute that is assigned to the user profile in your user pool\. Custom attributes are listed after standard attributes\.

   1. Select an attribute from the **SAML attribute** column\. This will be the attribute passed from the provider directory\.

      Your identity provider might offer sample SAML assertions for reference\. Some identity providers use simple names, such as `email`, while others use URL\-formatted attribute names similar to:

      ```
      http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
      ```

   1. To map additional attributes between your IDP and Amazon Cognito, choose **Add another attribute**\.

1. Choose **Save changes**\.

------

## Specifying identity provider attribute mappings for your user pool \(AWS CLI and AWS API\)<a name="cognito-user-pools-specifying-attribute-mapping-cli-api"></a>

Use the following commands to specify identity provider attribute mappings for your user pool\.

**To specify attribute mappings at provider creation time**
+ AWS CLI: `aws cognito-idp create-identity-provider`

  Example with metadata file: `aws cognito-idp create-identity-provider --user-pool-id <user_pool_id> --provider-name=SAML_provider_1 --provider-type SAML --provider-details file:///details.json --attribute-mapping email=http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

  Where `details.json` contains:

  ```
  { 
      "MetadataFile": "<SAML metadata XML>"
  }
  ```
**Note**  
If the *<SAML metadata XML>* contains any quotations \(`"`\), they must be escaped \(`\"`\)\.

  Example with metadata URL: `aws cognito-idp create-identity-provider --user-pool-id <user_pool_id> --provider-name=SAML_provider_1 --provider-type SAML --provider-details MetadataURL=<metadata_url> --attribute-mapping email=http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
+ AWS API: [CreateIdentityProvider](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateIdentityProvider.html)

**To specify attribute mappings for an existing identity provider**
+ AWS CLI: `aws cognito-idp update-identity-provider`

  Example: `aws cognito-idp update-identity-provider --user-pool-id <user_pool_id> --provider-name <provider_name> --attribute-mapping email=http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`
+ AWS API: [UpdateIdentityProvider](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateIdentityProvider.html)

**To get information about attribute mapping for a specific identity provider**
+ AWS CLI: `aws cognito-idp describe-identity-provider`

  Example: `aws cognito-idp describe-identity-provider --user-pool-id <user_pool_id> --provider-name <provider_name>`
+ AWS API: [DescribeIdentityProvider](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DescribeIdentityProvider.html)