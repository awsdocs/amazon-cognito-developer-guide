# Configuring Attribute Mapping for Your User Pool<a name="cognito-user-pools-attribute-mapping"></a>

**Note**  
The **Attribute mapping** tab appears only when you're editing an existing user pool\.

On the **Attribute mapping** tab, you can map identity provider \(IdP\) attributes or assertions to user pool attributes\. For more information, see [Specifying Identity Provider Attribute Mappings for Your User Pool](cognito-user-pools-specifying-attribute-mapping.md)\.

**Note**  
Currently, only the Facebook `id`, Google `sub`, Login with Amazon `user_id`, and Sign in with Apple `sub` attributes can be mapped to the Amazon Cognito User Pools `username` attribute\.

**Note**  
The attribute in the user pool must be large enough to fit the values of the mapped identity provider attributes, or an error occurs when users sign in\. Custom attributes should be set to the maximum 2048 character size if mapped to identity provider tokens\.   
You must create mappings for any attributes that are required for your user pool\.

**To specify a social identity provider attribute mapping for your user pool**

1. Choose the **Facebook**, **Google**, **Amazon**, or **Apple** tab\.

1. For each attribute you need to map, perform the following steps:

   1. Choose the **Capture** check box\.

   1. In the **User pool attribute** field, choose the user pool attribute to map the social identity provider attribute to from the drop\-down list\.

   1. For Facebook, Google, and Login with Amazon, if you need more attributes, choose either **Add Facebook attribute**, **Add Google attribute** or **Add Amazon attribute**, and then perform the following steps:
**Note**  
Sign in with Apple does not provide additional attributes at this time\.

      1. In the **Facebook attribute**, **Google attribute** or **Amazon attribute** field, enter the name of the attribute to be mapped\.

      1. For **User pool attribute**, choose the user pool attribute you want to map to the social identity provider attribute to from the drop\-down list\.

   1. Choose **Save changes**\.

**To specify a SAML provider attribute mapping for your user pool**

1. Choose the **SAML** tab\.

1. For each attribute you need to map, perform the following steps:

   1. Choose **Add SAML attribute**\.

   1. In the **SAML attribute** field, enter the name of the SAML attribute to be mapped\.

   1. For **User pool attribute**, choose the user pool attribute you want to map to the SAML attribute from the drop\-down list\.

   1. Choose **Save changes**\.