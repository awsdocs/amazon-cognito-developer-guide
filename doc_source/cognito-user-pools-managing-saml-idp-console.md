# Creating and Managing a SAML Identity Provider for a User Pool \(AWS Management Console\)<a name="cognito-user-pools-managing-saml-idp-console"></a>

You can use the AWS Management Console to create and delete SAML identity providers\.

Before you create a SAML identity provider, you need the SAML metadata document that you get from the third\-party identity provider \(IdP\)\. For instructions on how to get or generate the required SAML metadata document, see [Integrating Third\-Party SAML Identity Providers with Amazon Cognito User Pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.

**To configure a SAML 2\.0 identity provider in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose **SAML** to open the SAML dialog\.

1. Under **Metadata document** upload a metadata document from your SAML IdP\. You can also enter a URL that points to the metadata document\. For more information, see [Integrating Third\-Party SAML Identity Providers with Amazon Cognito User Pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.
**Note**  
We recommend that you provide the endpoint URL if it is a public endpoint, rather than uploading a file, because this allows Amazon Cognito to refresh the metadata automatically\. Typically metadata refresh happens every 6 hours or before the metadata expires, whichever is earlier\.

1. Enter your SAML **Provider name**\. For more information on SAML naming see [Choosing SAML Identity Provider Names](cognito-user-pools-managing-saml-idp-naming.md)\.

1. Enter any optional SAML **Identifiers** you want to use\.

1. Select **Enable IdP sign out flow** if you want your user to be logged out from the SAML IdP when logging out from Amazon Cognito\.

   Enabling this flow sends a signed logout request to the SAML IdP when the [LOGOUT Endpoint](logout-endpoint.md) is called\.

   Configure this endpoint for consuming logout responses from your IdP\. This endpoint uses post binding\.

   ```
   https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/logout
   ```
**Note**  
If this option is selected and your SAML identity provider expects a signed logout request, you will also need to configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and logout your user from the Amazon Cognito session\.

1. Choose **Create provider**\.

1. On the **Attribute mapping** tab, add mappings for at least the required attributes, typically `email`, as follows:

   1. Type the SAML attribute name as it appears in the SAML assertion from your identity provider\. If your identity provider offers sample SAML assertions, that might help you to find the name\. Some identity providers use simple names, such as `email`, while others use names similar to this:

      ```
      http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
      ```

   1. Choose the destination user pool attribute from the drop\-down list\.

1. Choose **Save changes**\.

1. Choose **Go to summary**\.

**Note**  
If you see `InvalidParameterException` while creating a SAML identity provider with an HTTPS metadata endpoint URL, for example, "Error retrieving metadata from *<metadata endpoint>*," make sure that the metadata endpoint has SSL correctly set up and that there is a valid SSL certificate associated with it\.

**To set up the SAML IdP to add a user pool as a relying party**
+ The user pools service provider URN is: `urn:amazon:cognito:sp:<user_pool_id>`\. Amazon Cognito issues the `AuthnRequest` to SAML IdP to issue a SAML assertion with audience restriction to this URN\. Your IdP uses the following POST binding endpoint for the IdP\-to\-SP response message: `https://<domain_prefix>.auth.<region>.amazoncognito.com/saml2/idpresponse`\.
+ Make sure your SAML IdP populates `NameID` and any required attributes for your user pool in the SAML assertion\. `NameID` is used for uniquely identifying your SAML federated user in the user pool\. Use persistent SAML Name ID format\.

**To set up the SAML IdP to add a signing certificate**
+ To get the certificate containing the public key which will be used by the identity provider to verify the signed logout request, choose **Show signing certificate** under **Active SAML Providers** on the **SAML** dialog under **Identity providers** on the **Federation** console page\.

**To delete a SAML provider**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, choose **Manage your User Pools**, and choose the user pool you want to edit\.

1. Choose **Identity providers** from the **Federation** console page\.

1. Choose **SAML** to display the SAML identity providers\.

1. Select the check box next to the provider to be deleted\.

1. Choose **Delete provider**\.