# Adding SAML identity providers to a user pool<a name="cognito-user-pools-saml-idp"></a>

You can choose to have your web and mobile app users sign in through a SAML identity provider \(IdP\) such as [Microsoft Active Directory Federation Services \(ADFS\)](https://msdn.microsoft.com/en-us/library/bb897402.aspx), or [Shibboleth](http://www.shibboleth.net/)\. You must choose a SAML IdP which supports the [SAML 2\.0 standard](http://saml.xml.org/saml-specifications)\.

With the built\-in hosted web UI, Amazon Cognito provides token handling and management for all authenticated users\. This way, your backend systems can standardize on one set of user pool tokens\. You can create and manage a SAML IdP in the AWS Management Console through the AWS CLI or with Amazon Cognito API calls\. To get started with the console see [Adding sign\-in through SAML\-based identity providers to a user pool with the AWS Management Console](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-saml-2-0-idp.html)\.

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

You need to update your SAML IdP and configure your user pool to support the provider\. See the documentation for your SAML IdP for information about how to add your user pool as a relying party or application for your SAML 2\.0 IdP\.

Amazon Cognito and your SAML IdP maintain session information with a `relayState` parameter\.

**Things to know about `relayState`**

1. Amazon Cognito supports `relayState` values greater than 80 bytes\. While SAML specifications state that the `relayState` value "Must not exceed 80 bytes in length”, current industry practice often deviates from this behavior\. As a consequence, rejecting `relayState` values greater than 80 bytes will break many standard SAML provider integrations\.

1. The RelayState token is an opaque reference to state information maintained by Amazon Cognito\. Amazon Cognito doesn't guarantee the contents of the `relayState` parameter\. Don't parse its contents such that your app depends on the result\. For more information, see the [SAML 2\.0 specification](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html)\.

You also need to provide an assertion consumer endpoint to your SAML identity provider\. Configure the following endpoint in your user pool domain for SAML 2\.0 POST binding in your SAML identity provider\. See [Configuring a user pool domain](cognito-user-pools-assign-domain.md) for more information about user pool domains\.

```
https://Your user pool domain/saml2/idpresponse
With an Amazon Cognito domain:
https://<yourDomainPrefix>.auth.<region>.amazoncognito.com/saml2/idpresponse
With a custom domain:
https://Your custom domain/saml2/idpresponse
```

You can find your domain prefix and the region value for your user pool on the **Domain name** tab of the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

**Note**  
The Amazon Cognito `idpresponse` endpoint doesn't accept encrypted SAML assertions\.

For some SAML IdPs, you also need to provide the SP `urn` / Audience URI / SP Entity ID, in the form:

```
urn:amazon:cognito:sp:<yourUserPoolID>
```

You can find your user pool ID on the **General settings** tab in the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

You should also configure your SAML IdP to provide attribute values for any attributes that are required in your user pool\. For example, `email` is a typical required attribute for user pools\. Thus, the SAML IdP should provide an `email` value \(claim\) in the SAML assertion\.

Amazon Cognito user pools support SAML 2\.0 federation with post\-binding endpoints\. This eliminates the need for your app to retrieve or parse SAML assertion responses, because the user pool directly receives the SAML response from your IdP through a user agent\. Your user pool acts as a service provider \(SP\) on behalf of your application\. Amazon Cognito supports SP\-initiated single sign\-on \(SSO\) as described in section 5\.1\.2 of the [SAML V2\.0 Technical Overview](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02.html#5.1.2.SP-Initiated%20SSO:%20%20Redirect/POST%20Bindings|outline)\.

**Amazon Cognito SAML requests**

When the [Authorize endpoint](authorization-endpoint.md) redirects your user to your IdP sign\-in page, Amazon Cognito includes a *SAML request* in a URL parameter of the `HTTP GET` request\. A SAML request contains information about your user pool and your `saml2/idpresponse` endpoint\. Amazon Cognito doesn't sign SAML requests\.

Amazon Cognito signs *logout* requests that your user passes to the logout endpoint of your IdP\. To establish trust with these logout requests, you can provide your IdP with a copy of your user pool SAML 2\.0 signing certificate that you downloaded in the Amazon Cognito console **Sign\-in experience** tab\.

**Topics**
+ [SAML user pool IdP authentication flow](cognito-user-pools-saml-idp-authentication.md)
+ [Choosing SAML identity provider names](cognito-user-pools-managing-saml-idp-naming.md)
+ [Creating and managing a SAML identity provider for a user pool \(AWS Management Console\)](cognito-user-pools-managing-saml-idp-console.md)
+ [Creating and managing a SAML identity provider for a user pool \(AWS CLI and AWS API\)](cognito-user-pools-managing-saml-idp-cli-api.md)
+ [Integrating third\-party SAML identity providers with Amazon Cognito user pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)
+ [SAML session initiation in Amazon Cognito user pools](cognito-user-pools-SAML-session-initiation.md)