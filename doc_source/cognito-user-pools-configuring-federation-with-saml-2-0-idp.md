# Step 4\. Add Sign\-in with a SAML Identity Provider to a User Pool \(Optional\)<a name="cognito-user-pools-configuring-federation-with-saml-2-0-idp"></a>

You can enable your app users to sign in through a SAML identity provider \(IdP\)\. Whether your users sign in directly or through a third party, all users have a profile in the user pool\. Skip this step if you don't want to add sign in through a SAML identity provider\. 

You need to update your SAML identity provider and configure your user pool\. See the documentation for your SAML identity provider for information about how to add your user pool as a relying party or application for your SAML 2\.0 identity provider\.

You also need to provide an assertion consumer endpoint to your SAML identity provider\. Configure this endpoint for SAML 2\.0 POST binding in your SAML identity provider:

```
https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/idpresponse
```

You can find your domain prefix and the region value for your user pool on the **Domain name** tab of the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

For some SAML identity providers, you also need to provide the SP `urn` / Audience URI / SP Entity ID, in the form:

```
      
urn:amazon:cognito:sp:<yourUserPoolID>
```

You can find your user pool ID on the **General settings** tab in the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

You should also configure your SAML identity provider to provide attribute values for any attributes that are required in your user pool\. Typically, `email` is a required attribute for user pools\. In that case, the SAML identity provider should provide an `email` value \(claim\) in the SAML assertion\.

Amazon Cognito user pools support SAML 2\.0 federation with post\-binding endpoints\. This eliminates the need for your app to retrieve or parse SAML assertion responses, because the user pool directly receives the SAML response from your identity provider via a user agent\.

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

For more information, see [Adding SAML Identity Providers to a User Pool](cognito-user-pools-saml-idp.md)\.