# Choosing SAML Identity Provider Names<a name="cognito-user-pools-managing-saml-idp-naming"></a>

You need to choose names for your SAML providers\. The string format is \[\\w\\s\+=\.@\-\]\+ and can be up to 40 characters long\.

You can also optionally choose identifiers for your SAML providers\. An identifier uniquely resolves to an identity provider associated with your user pool\. Typically each identifier corresponds to a domain that belongs to the company that the SAML IdP represents\. For a multitenant app that can be used by different companies, identifiers can be used to redirect users to the correct IdP\. Since there can be multiple domains owned by the same company, you can provide multiple identifiers\.

You can associate up to 50 identifiers with each SAML provider\. Identifiers must be unique across the identity provider\.

For example, suppose that you built an app that can be used by employees of two different companies, A and B\. Company A owns `domainA.com` and `domainA.co.uk`; Company B owns `domainB.com`\. Suppose further that you set up two IdPs, one for each company: 
+ For IdP A, you can define identifiers `DomainA.com` and `DomainA.co.uk`\.
+ For IdP B, you can define identifier `DomainB.com`\.

In your application, you can prompt users to enter their email addresses\. By deriving the domain from the email address, you can redirect the user to the correct IdP by providing the domain in the `IdPIdentifier` in the call to the `/authorize` endpoint\. For example, if a user enters `bob@domain1.co.uk`, the user is redirected to IdP A\.

The sign\-in page hosted by Amazon Cognito parses the email address automatically to derive the information\. It parses the email domain from email and uses it as `IdPIdentifier` when calls the `/authorize` endpoint\. 
+ If you have multiple SAML IdPs and you specify an `IdPIdentifier` value for all of them, you will see a box to enter an email address on the hosted page\.
+ If you have multiple IdPs, and you do not specify an `IdPIdentifier` value for any of them, the hosted page will show a list of IdPs\.

If you're building your own UI, you should parse the domain name so that it matches the `IdPIdentifiers` that are provided during the IdP setup\. For more information about IdP setup, see [Configuring Identity Providers for Your User Pool](cognito-user-pools-identity-provider.md)\.