# Step 4\. Add sign\-in with a SAML identity provider to a user pool \(optional\)<a name="cognito-user-pools-configuring-federation-with-saml-2-0-idp"></a>

You can enable your app users to sign in through a SAML identity provider \(IdP\)\. Whether your users sign in directly or through a third party, all users have a profile in the user pool\. Skip this step if you don't want to add sign in through a SAML identity provider\. 

You need to update your SAML identity provider and configure your user pool\. See the documentation for your SAML identity provider for information about how to add your user pool as a relying party or application for your SAML 2\.0 identity provider\.

You also need to provide an assertion consumer endpoint to your SAML identity provider\. Configure this endpoint for SAML 2\.0 POST binding in your SAML identity provider:

```
https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/idpresponse
```

You can find your domain prefix and the region value for your user pool on the **Domain name** tab of the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

For some SAML identity providers, you also need to provide the SP `urn` / Audience URI / SP Entity ID, in the format:

```
urn:amazon:cognito:sp:<yourUserPoolID>
```

You can find your user pool ID on the **General settings** tab in the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

You should also configure your SAML identity provider to provide attribute values for any attributes that are required in your user pool\. Typically, `email` is a required attribute for user pools\. In that case, the SAML identity provider should provide an `email` value \(claim\) in the SAML assertion\.

Amazon Cognito user pools support SAML 2\.0 federation with post\-binding endpoints\. This eliminates the need for your app to retrieve or parse SAML assertion responses, because the user pool directly receives the SAML response from your identity provider through a user agent\.

------
#### [ Original console ]

**To configure a SAML 2\.0 identity provider in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose **SAML** to open the SAML dialog\.

1. Under **Metadata document**, upload a metadata document from your SAML IdP\. You can also enter a URL that points to the metadata document\. For more information, see [Integrating third\-party SAML identity providers with Amazon Cognito user pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.
**Note**  
We recommend that you provide the endpoint URL if it is a public endpoint, rather than uploading a file, because this allows Amazon Cognito to refresh the metadata automatically\. Typically metadata refresh happens every 6 hours or before the metadata expires, whichever is earlier\.

1. Enter your SAML **Provider name**\. For more information on SAML naming see [Choosing SAML identity provider names](cognito-user-pools-managing-saml-idp-naming.md)\.

1. Enter any optional SAML **Identifiers** you want to use\.

1. Select **Enable IdP sign out flow** if you want your user to be logged out from both Amazon Cognito and the SAML IdP when they log out from your user pool\.

   Enabling this flow sends a signed logout request to the SAML IdP when the [Logout\-endpoint](https://docs.aws.amazon.com/cognito/latest/developerguide/logout-endpoint.html) is called\.

   Configure this endpoint for consuming logout responses from your IdP\. This endpoint uses post binding\.

   ```
   https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/logout
   ```
**Note**  
If this option is selected and your SAML identity provider expects a signed log out request, you will also need to configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and log your user out of the Amazon Cognito session\.

1. Choose **Create provider**\.

1. On the **Attribute mapping** tab, add mappings for at least the required attributes, typically `email`, as follows:

   1. Type the SAML attribute name as it appears in the SAML assertion from your identity provider\. Your identity provider might offer sample SAML assertions for reference\. Some identity providers use simple names, such as `email`, while others use URL\-formatted attribute names, such as the following example::

      ```
      http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
      ```

   1. Choose the destination user pool attribute from the drop\-down list\.

1. Choose **Save changes**\.

1. Choose **Go to summary**\.

------
#### [ New console ]

**To configure a SAML 2\.0 identity provider in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and select **Add an identity provider**\.

1. Choose a **SAML** social identity provider\.

1. Enter **Identifiers** separated by commas\. An identifier tells Amazon Cognito it should check the email address a user enters when they sign in, and then direct them to the provider that corresponds to their domain\.

1. Choose **Add sign\-out flow** if you want Amazon Cognito to send signed sign\-out requests to your provider when a user logs out\. You must configure your SAML 2\.0 identity provider to send sign\-out responses to the `https://<your Amazon Cognito domain>/saml2/logout` endpoint that is created when you configure the hosted UI\. The `saml2/logout` endpoint uses POST binding\.
**Note**  
If this option is selected and your SAML identity provider expects a signed logout request, you will also need to configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and logout your user from the Amazon Cognito session\.

1. Choose a **Metadata document source**\. If your identity provider offers SAML metadata at a public URL, you can choose **Metadata document URL**and enter that public URL\. Otherwise, choose **Upload metadata document** and select a metadata file you downloaded from your provider earlier\.
**Note**  
We recommend that you enter a metadata document URL if your provider has a public endpoint, rather than uploading a file; this allows Amazon Cognito to refresh the metadata automatically\. Typically, metadata refresh happens every 6 hours or before the metadata expires, whichever is earlier\.

1. Select **Map attributes between your SAML provider and your app** to map SAML provider attributes to the user profile in your user pool\. Include your user pool required attributes in your attribute map\. 

   For example, when you choose the **User pool attribute** `email`, enter the SAML attribute name as it appears in the SAML assertion from your identity provider\. Your identity provider might offer sample SAML assertions for reference\. Some identity providers use simple names, such as `email`, while others use URL\-formatted attribute names, such as the following example:

   ```
   http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
   ```

1. Choose **Create**\.

------

For more information, see [Adding SAML identity providers to a user pool](cognito-user-pools-saml-idp.md)\.