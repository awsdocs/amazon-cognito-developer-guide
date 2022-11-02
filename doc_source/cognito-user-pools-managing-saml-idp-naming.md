# Choosing SAML identity provider names<a name="cognito-user-pools-managing-saml-idp-naming"></a>

Choose names for your SAML providers\. The string can be up to 32 characters long and must match the following regular expression:

`[\p{L}\p{M}\p{S}\p{N}\p{P}\p{Z}]+|[^_\p{Z}][\p{L}\p{M}\p{S}\p{N}\p{P}][^_\p{Z}]+`

You can also choose identifiers for your SAML providers\. An identifier uniquely resolves to an identity provider \(IdP\) associated with your user pool\. Typically, each identifier corresponds to an organization domain that the SAML IdP represents\. For a multi\-tenant app that multiple organizations share, you can use identifiers to redirect users to the correct IdP\. Because the same organization can own multiple domains, you can provide multiple identifiers\. To sign in your users with an identifier, direct their sessions to the [Authorize endpoint](authorization-endpoint.md) for your app client with an *idp\_identifier* parameter\.

You can associate up to 50 identifiers with each SAML provider\. Identifiers must be unique across the user pool\.

You can create links to your Amazon Cognito user pool `/authorize` endpoint that silently redirect your user to the sign\-in page for your IdP\. Include an identifier as an `idp_identifier` parameter in a request to your user pool `/authorize` endpoint\. Alternatively, you can populate the parameter `identity_provider` with the name of your IdP\. Use the following URL format:

`https://your_Amazon_Cognito_userpool_domain/authorize?response_type=code&identity_provider=your-SAML-IdP-name&client_id=your-client-id&redirect_uri=https://your_application_redirect_url`

After a user signs in with your SAML IdP, they transparently submit the SAML response to your `saml2/idpresponse` endpoint\. Amazon Cognito replies to the IdP response with a redirect to your app client callback URL\. The user hasn't seen any interaction with your hosted UI\.

Create IdP identifiers in a domain format to collect user email addresses and direct them to the sign\-in page for the IdP that corresponds to their domain\. For example, suppose that you built an app for employees of two different companies to use\. The first company, AnyCompany A, owns `exampleA.com` and `exampleA.co.uk`\. The second company, AnyCompany B, owns `exampleB.com`\. For this example, you have set up two IdPs, one for each company, as follows: 
+ For IdP A, you define identifiers `exampleA.com` and `exampleA.co.uk`\.
+ For IdP B, you define identifier `exampleB.com`\.

In your app, you can prompt users to enter their email addresses\. Your app derives the users' domain from their email address and redirects them to the correct IdP by providing the domain in the `idp_identifier` parameter in the call to the `/authorize` endpoint\. For example, if a user enters `bob@exampleA.co.uk`, the user is redirected to IdP A\.

The Amazon Cognito hosted UI can collect an email address for federated users and parse the domain\. To do this, assign at least one identifier to each SAML IdP that you have assigned to your app client\. If you have successfully assigned identifiers, your hosted UI sign\-in page looks like the following image\.

![\[An Amazon Cognito hosted UI sign-in page displaying local user sign-in and a prompt for a federated user to enter an email address.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

To use this strategy, you must use domains as your IdP identifiers\. Amazon Cognito extracts the email domain from the email address that your user enters\. Amazon Cognito then adds the email domain as an `IdPIdentifier` parameter in a request to the `/authorize` endpoint\. Your user sees an IdP sign\-in page after they choose the **Sign in** button shown in the previous image\.
+ If you assign only one SAML IdP to your app client, the hosted UI sign\-in page displays a button to sign in with that IdP\.
+ If you assign an identifier to every SAML IdP that you activate for your app client, a box to enter an email address appears in the hosted UI sign\-in page\.
+ If you have multiple IdPs and you do not assign an identifier to all of them, the hosted page will contain a list of IdPs\.

If you have a custom UI, parse the domain name so that it matches the identifiers that you assigned when you added the IdP\. For more information about IdP setup, see [Configuring identity providers for your user pool](cognito-user-pools-identity-provider.md)\.