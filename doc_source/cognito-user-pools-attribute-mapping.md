# Configuring attribute mapping for your user pool<a name="cognito-user-pools-attribute-mapping"></a>

------
#### [ Original console ]

**Note**  
The **Attribute mapping** tab only appears when you're editing an existing user pool\.

On the **Attribute mapping** tab, you can map identity provider \(IdP\) attributes or assertions to user pool attributes\. For more information, see [Specifying identity provider attribute mappings for your user pool](cognito-user-pools-specifying-attribute-mapping.md)\.

**Note**  
Currently, only the Facebook `id`, Google `sub`, Login with Amazon `user_id`, and Sign in with Apple `sub` attributes can be mapped to the Amazon Cognito user pools `username` attribute\.

**Note**  
The attribute in the user pool must be large enough to contain the values of the mapped identity provider attributes, or an error will occur when users sign in\. Custom attributes should be set to the maximum 2048 character size if mapped to identity provider tokens\.   
You must create mappings for any attributes that are required for your user pool\.

**To specify a social identity provider attribute mapping for your user pool**

1. Choose the **Facebook**, **Google**, **Amazon**, or **Apple** tab\.

1. For each attribute you need to map, complete the following steps:

   1. Select the **Capture** check box\.

   1. In the **User pool attribute** field, select the user pool attribute from the drop\-down list to map to the social identity provider attribute\.

   1. For Facebook, Google, and Login with Amazon, if you need more attributes, choose either **Add Facebook attribute**, **Add Google attribute** or **Add Amazon attribute**, and then perform the following steps:
**Note**  
Sign in with Apple does not provide additional attributes at this time\.

      1. In the **Facebook attribute**, **Google attribute** or **Amazon attribute** field, enter the name of the attribute to map\.

      1. For **User pool attribute**, choose the user pool attribute from the drop\-down list that you want to map to the social identity provider attribute\.

   1. Choose **Save changes**\.

**To specify a SAML provider attribute mapping for your user pool**

1. Choose the **SAML** tab\.

1. For each attribute you need to map, complete the following steps:

   1. Choose **Add SAML attribute**\.

   1. In the **SAML attribute** field, enter the name of the SAML attribute to map\.

   1. For **User pool attribute**, choose the user pool attribute from the drop\-down list that you want to map to the SAML attribute\.

   1. Choose **Save changes**\.

------
#### [ New console ]

On the **Attribute mapping** tab, you can map identity provider \(IdP\) attributes or assertions to user pool attributes\. For more information, see [Specifying identity provider attribute mappings for your user pool](cognito-user-pools-specifying-attribute-mapping.md)\.

**Note**  
Currently, only the Facebook `id`, Google `sub`, Login with Amazon `user_id`, and Sign in with Apple `sub` attributes can be mapped to the Amazon Cognito User Pools `username` attribute\.

**Note**  
The attribute in the user pool must be large enough to contain the values of the mapped identity provider attributes, or an error will occur when users sign in\. Custom attributes should be set to the maximum 2048 character size if mapped to identity provider tokens\.   
You must create mappings for any attributes that are required for your user pool\.

**To specify a social identity provider attribute mapping for your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools** from the navigation menu\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in**\.

1. Choose **Add an identity provider**, or choose the **Facebook**, **Google**, **Amazon** or **Apple** identity provider you have configured\. Locate **Attribute mapping**, and choose **Edit**\. For more information about adding a social identity provider, see [Adding social identity providers to a user pool](cognito-user-pools-social-idp.md)\.

1. For each attribute you need to map, complete the following steps:

   1. Select an attribute from the **User pool attribute** column\. This is the attribute that is assigned to the user profile in your user pool\. Custom attributes are listed after standard attributes\.

   1. Select an attribute from the ***<provider>* attribute** column\. This will be the attribute passed from the provider directory\. Known attributes from the social provider are provided in a drop\-down list\.

   1. To map additional attributes between your IdP and Amazon Cognito, choose **Add another attribute**\.

1. Choose **Save changes**\.

**To specify a SAML provider attribute mapping for your user pool**

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

   1. To map additional attributes between your IdP and Amazon Cognito, choose **Add another attribute**\.

1. Choose **Save changes**\.

------