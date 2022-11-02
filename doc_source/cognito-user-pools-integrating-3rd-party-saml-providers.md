# Integrating third\-party SAML identity providers with Amazon Cognito user pools<a name="cognito-user-pools-integrating-3rd-party-saml-providers"></a>

To configure third\-party SAML 2\.0 identity provider \(IdP\) solutions to work with federation for Amazon Cognito user pools, you must enter the following redirect URL: `https://Your user pool domain/saml2/idpresponse`\. If your user pool has an Amazon Cognito domain, you can find your user pool domain path in the **App integration** tab of your user pool in the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

**Note**  
Any SAML IdPs that you created in a user pool during the public beta before August 10, 2017 have redirect URLs of `https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/login/redirect`\. If you have one of these SAML identity providers from the public beta in your user pool, you must either:  
Replace it with a new one that uses the new redirect URL\.
Update the configuration in your SAML IdP to accept both the old and new redirect URLs\.
All SAML IdPs in Amazon Cognito switch to the new URLs\. The old URL will stop working on October 31, 2017\.

For some SAML IdPs, provide the `urn` / Audience URI / SP Entity ID, in the form `urn:amazon:cognito:sp:<yourUserPoolID>`\. You can find your user pool ID on the **General settings** tab in the Amazon Cognito console\.

You must also configure your SAML IdP to provide attributes values for any attributes required in your user pool\. Typically, `email` is a required attribute for user pools, in which case the SAML IdP must provide an `email` value \(claim\) in the SAML assertion\.

**Note**  
Amazon Cognito will not accept an emoji that your IdP passes as an attribute value\. You can Base64 encode the emoji to pass it as text, and then decode it in your app\.  
In the following example, the attribute claim will not be accepted:  

```
<saml2:Attribute Name="Name" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:unspecified">
  <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xsd:string">😐</saml2:AttributeValue>
</saml2:Attribute>
```
In contrast to the preceding example, the following attribute claim will be accepted:  

```
<saml2:Attribute Name="Name" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:unspecified">
  <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xsd:string">8J+YkA==</saml2:AttributeValue>
</saml2:Attribute>
```

The following links help you configure third\-party SAML 2\.0 IdP solutions to work with federation for Amazon Cognito user pools\.

**Note**  
Amazon Cognito includes IdP support, so you only need to go to the following provider sites to get the SAML metadata document\. You might see further instructions on the provider website about integrating with AWS, but you won't need those\.


| Solution | More information | 
| --- | --- | 
| Microsoft Active Directory Federation Services \(AD FS\) | You can download the SAML metadata document for your ADFS federation server from the following address: https://<yourservername>/FederationMetadata/2007\-06/FederationMetadata\.xml\. | 
| Okta | Once you have configured your Amazon Cognito user pool as an application in Okta, you can find the metadata document in the Admin section of the Okta dashboard\. Choose the application, select the Sign On section, and look under the Settings for SAML\. The URL should look like https://<app\-domain>\.oktapreview\.com/app/<application\-ID>/sso/saml/metadata\. | 
| Auth0 | The metadata download document is obtained from the Auth0 dashboard\. Choose Clients, and then choose Settings\. Scroll down, choose Show Advanced Settings, and then look for your SAML Metadata URL\. It should look like https://<your\-domain\-prefix>\.auth0\.com/samlp/metadata/<your\-Auth0\-client\-ID>\. | 
| Ping Identity | For PingFederate, you can find instructions for downloading a metadata XML file in [Provide general SAML metadata by file](https://documentation.pingidentity.com/pingfederate/pf81/index.shtml#task_toExportSelectedMetadata.html#task_toExportSelectedMetadata)\. | 