# Adding SAML Identity Providers to a User Pool<a name="cognito-user-pools-saml-idp"></a>

You can enable your web and mobile app users to sign in through a SAML identity provider \(IdP\) such as [Microsoft Active Directory Federation Services \(ADFS\)](https://msdn.microsoft.com/en-us/library/bb897402.aspx), or [Shibboleth](http://www.shibboleth.net/)\. Choose a SAML identity provider that supports the [SAML 2\.0 standard](http://saml.xml.org/saml-specifications)\.

With the built\-in hosted web UI, Amazon Cognito provides token handling and management for all authenticated users, so your backend systems can standardize on one set of user pool tokens\. You can create and manage a SAML IdP in the AWS Management Console, with the AWS CLI, or using Amazon Cognito API calls\. To get started with the console see [Adding sign\-in through SAML\-based identity providers to a user pool with the AWS Management Console](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-saml-2-0-idp.html)\.

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

You need to update your SAML identity provider and configure your user pool to support it\. See the documentation for your SAML identity provider for information about how to add your user pool as a relying party or application for your SAML 2\.0 identity provider\.

**Note**  
Cognito supports `relayState` values larger than 80 bytes\. Whilst SAML specifications state that the `relayState` value "MUST NOT exceed 80 bytes in length”, current industry practice often deviates from this behavior\. As a consequence, rejecting `relayState` more than 80 bytes will break many standard SAML provider integrations\.

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

Amazon Cognito user pools support SAML 2\.0 federation with post\-binding endpoints\. This eliminates the need for your app to retrieve or parse SAML assertion responses, because the user pool directly receives the SAML response from your identity provider via a user agent\. Your user pool acts as a service provider \(SP\) on behalf of your application\. Amazon Cognito supports SP\-initiated single sign\-on \(SSO\) as described in section 5\.1\.2 of the [SAML V2\.0 Technical Overview](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02.html#5.1.2.SP-Initiated%20SSO:%20%20Redirect/POST%20Bindings|outline)\.

**Topics**
+ [SAML User Pool IdP Authentication Flow](cognito-user-pools-saml-idp-authentication.md)
+ [Choosing SAML Identity Provider Names](cognito-user-pools-managing-saml-idp-naming.md)
+ [Creating and Managing a SAML Identity Provider for a User Pool \(AWS Management Console\)](cognito-user-pools-managing-saml-idp-console.md)
+ [Creating and Managing a SAML Identity Provider for a User Pool \(AWS CLI and AWS API\)](cognito-user-pools-managing-saml-idp-cli-api.md)
+ [Integrating Third\-Party SAML Identity Providers with Amazon Cognito User Pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)