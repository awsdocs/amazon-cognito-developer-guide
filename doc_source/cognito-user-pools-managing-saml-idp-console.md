# Creating and managing a SAML identity provider for a user pool \(AWS Management Console\)<a name="cognito-user-pools-managing-saml-idp-console"></a>

You can use the AWS Management Console to create and delete SAML identity providers \(IdPs\)\.

Before you create a SAML IdP, you will need the SAML metadata document that you get from the third\-party IdP\. For instructions on how to get or generate the required SAML metadata document, see [Integrating third\-party SAML identity providers with Amazon Cognito user pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.

------
#### [ Original console ]

**To configure a SAML 2\.0 IdP in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose **SAML** to open the SAML dialog\.

1. Under **Metadata document**, upload a metadata document from your SAML IdP\. You can also enter a URL that points to the metadata document\. For more information, see [Integrating third\-party SAML identity providers with Amazon Cognito user pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.
**Note**  
We recommend that you provide the endpoint URL if it is a public endpoint, rather than uploading a file, because this allows Amazon Cognito to refresh the metadata automatically\. Typically, metadata refresh happens every 6 hours or before the metadata expires, whichever is earlier\.

1. Enter your SAML **Provider name**\. For more information on SAML naming see [Choosing SAML identity provider names](cognito-user-pools-managing-saml-idp-naming.md)\.

1. Enter any optional SAML **Identifiers** you want to use\.

1. Select **Enable IdP sign out flow** if you want your user to be logged out from both Amazon Cognito and the SAML IdP when they log out from your user pool\.

   Enabling this flow sends a signed log out request to the SAML IdP when the [Logout\-endpoint](https://docs.aws.amazon.com/cognito/latest/developerguide/logout-endpoint.html) is called\.

   Configure this endpoint for consuming log out responses from your IdP\. This endpoint uses post binding\.

   ```
   https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/logout
   ```
**Note**  
If this option is selected and your SAML IdP expects a signed logout request, you will also need to configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed log out request and log your user out from the Amazon Cognito session\.

1. Choose **Create provider**\.

1. On the **Attribute mapping** tab, add mappings for at least the required attributes, typically `email`, as follows:

   1. Enter the SAML attribute name as it appears in the SAML assertion from your IdP\. Your IdP might offer sample SAML assertions for reference\. Some IdPs use simple names, such as `email`, while others use URL\-formatted attribute names similar to:

      ```
      http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
      ```

   1. Choose the destination user pool attribute from the drop\-down list\.

1. Choose **Save changes**\.

1. Choose **Go to summary**\.

------
#### [ New console ]

**To configure a SAML 2\.0 IdP in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and choose **Add an identity provider**\.

1. Choose a **SAML** IdP\.

1. Enter **Identifiers** separated by commas\. An identifier tells Amazon Cognito it should check the email address a user enters when they sign in, and then direct them to the provider that corresponds to their domain\.

1. Choose **Add sign\-out flow** if you want Amazon Cognito to send signed sign\-out requests to your provider when a user logs out\. You must configure your SAML 2\.0 IdP to send sign\-out responses to the `https://<your Amazon Cognito domain>/saml2/logout` endpoint that is created when you configure the hosted UI\. The `saml2/logout` endpoint uses POST binding\.
**Note**  
If this option is selected and your SAML IdP expects a signed logout request, you will also need to configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and sign out your user from the Amazon Cognito session\.

1. Choose a **Metadata document source**\. If your IdP offers SAML metadata at a public URL, you can choose **Metadata document URL** and enter that public URL\. Otherwise, choose **Upload metadata document** and select a metadata file you downloaded from your provider earlier\.
**Note**  
We recommend that you enter a metadata document URL if your provider has a public endpoint, rather than uploading a file; this allows Amazon Cognito to refresh the metadata automatically\. Typically, metadata refresh happens every 6 hours or before the metadata expires, whichever is earlier\.

1. **Map attributes between your SAML provider and your app** to map SAML provider attributes to the user profile in your user pool\. Include your user pool required attributes in your attribute map\. 

   For example, when you choose **User pool attribute** `email`, enter the SAML attribute name as it appears in the SAML assertion from your IdP\. If your IdP offers sample SAML assertions, you can use these sample assertions to help you to find the name\. Some IdPs use simple names, such as `email`, while others use names similar to this:

   ```
   http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
   ```

1. Choose **Create**\.

------

**Note**  
If you see `InvalidParameterException` while creating a SAML IdP with an HTTPS metadata endpoint URL, for example, "Error retrieving metadata from *<metadata endpoint>*," make sure that the metadata endpoint has SSL correctly set up and that there is a valid SSL certificate associated with it\.

**To set up the SAML IdP to add a user pool as a relying party**
+ The user pools service provider URN is: `urn:amazon:cognito:sp:<user_pool_id>`\. Amazon Cognito issues the `AuthnRequest` to SAML IdP to issue a SAML assertion with audience restriction to this URN\. Your IdP uses the following POST binding endpoint for the IdP\-to\-SP response message: `https://<domain_prefix>.auth.<region>.amazoncognito.com/saml2/idpresponse`\.
+ Make sure your SAML IdP populates `NameID` and any required attributes for your user pool in the SAML assertion\. `NameID` is used for uniquely identifying your SAML federated user in the user pool\. Use persistent SAML Name ID format\.

**To set up the SAML IdP to add a signing certificate**
+ To get the certificate containing the public key which will be used by the IdP to verify the signed logout request, choose **Show signing certificate** under **Active SAML Providers** on the **SAML** dialog under **Identity providers** on the **Federation** console page\.

You can delete any SAML provider you have set up in your user pool with the Amazon Cognito console\.

------
#### [ Original console ]

**To delete a SAML provider**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials

1. In the navigation pane, choose **Manage your User Pools**, and choose the user pool you want to edit\.

1. Choose **Identity providers** from the **Federation** console page\.

1. Choose **SAML** to display the SAML identity providers\.

1. Select the check box next to the provider to be deleted\.

1. Choose **Delete provider**\.

------
#### [ New console ]

**To delete a SAML provider**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, choose **User Pools**, and choose the user pool you want to edit\.

1. Choose the **Sign\-in experience** tab and locate **Federated sign\-in**\.

1. Select the radio button next to the SAML IdPs you wish to delete\.

1. When you are prompted to **Delete identity provider**, enter the name of the SAML provider to confirm deletion, and then choose **Delete**\.

------