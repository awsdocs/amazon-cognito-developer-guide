# SAML session initiation in Amazon Cognito user pools<a name="cognito-user-pools-SAML-session-initiation"></a>

Amazon Cognito supports service provider\-initiated \(SP\-initiated\) single sign\-on \(SSO\)\. Section 5\.1\.2 of the [ SAML V2\.0 Technical Overview](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0-cd-02.html#5.1.2.SP-Initiated%20SSO:%20%20Redirect/POST%20Bindings|outline) describes SP\-initiated SSO\. Amazon Cognito is the identity provider \(IdP\) to your app\. The app is the service provider \(SP\) that retrieves tokens for authenticated users\. However, when you use a third\-party IdP to authenticate users, Amazon Cognito is the SP\. Amazon Cognito users who authenticate with a SAML IdP must always first make a request to Amazon Cognito and redirect to the IdP for authentication\.

For some enterprise use cases, access to internal applications starts at a bookmark on a dashboard hosted by the enterprise IdP\. When a user selects a bookmark, the IdP generates a SAML response and sends it to the SP to authenticate the user with the application\.

Amazon Cognito doesn't support IdP\-initiated SSO\. Amazon Cognito can't verify that it has solicited the SAML response that it receives unless Amazon Cognito initiates authentication with a SAML request\.

## Bookmark Amazon Cognito apps in an enterprise dashboard<a name="bookmark-applications-in-idp-portal"></a>

You can create bookmarks in your SAML or [OIDC](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-oidc-idp.html) IdP dashboards that provide SSO access to web applications that authenticate with Amazon Cognito user pools\. You can link to Amazon Cognito in a way that doesn't require users to sign in with the hosted UI\. To do this, construct a bookmark link to the [Authorize endpoint](authorization-endpoint.md) of your Amazon Cognito user pool from the following template:

`https://your_Amazon_Cognito_userpool_domain/authorize?response_type=code&identity_provider=your-SAML-IdP-name&client_id=your-client-id&redirect_uri=https://your_application_redirect_url`

**Note**  
You can also use an `idp_identifier` parameter instead of an `identity_provider` parameter in your request to the authorization endpoint\. An IdP identifier is an alternative name you can configure when you create an identity provider in your user pool\. See [Choosing SAML identity provider names](cognito-user-pools-managing-saml-idp-naming.md)\.

When you use the appropriate parameters in your request to `/authorize`, Amazon Cognito silently begins the SP\-initiated sign\-in flow and redirects your user to sign in with your IdP\.

The following diagram shows an authentication flow that emulates IdP\-initiated SSO\. Your users can authenticate with Amazon Cognito from a link in your company portal\.

![\[Amazon Cognito SAML sign-in that begins at an enterprise application dashboard.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Upload or link a metadata document from your provider to add a SAML IdP in your user pool\. Create an app client that uses your SAML IdP for sign\-in, and has the URL for your app as an authorized callback URL\. For more information about app clients, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.

Before you try to use this solution to authenticate to your app, test SP\-initiated sign\-in to your app from your hosted UI\. For more information about how to configure a SAML IdP in Amazon Cognito, see [Integrating third\-party SAML identity providers with Amazon Cognito user pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.

After you meet these requirements, create a bookmark to your `/authorize` endpoint that includes either an `identity_provider` or an `idp_identifier` parameter\. The preceding diagram illustrates the process that follows\.

1. Your user signs in to the SSO IdP dashboard\. Enterprise applications that the user is authorized to access populate this dashboard\. 

1. Your user chooses the link to the application that authenticates with Amazon Cognito\. In many SSO portals, you can add a custom app link\. Any feature that you can use to create a link to a public URL in your SSO portal will work\.

1. Your custom app link in the SSO portal directs the user to the user pool [Authorize endpoint](authorization-endpoint.md)\. The link includes parameters for `response_type`, `client_id`, `redirect_uri` and `identity_provider`\. The `identity_provider` parameter is the name that you gave the IdP in your user pool\. You can also use an `idp_identifier` parameter instead of the `identity_provider` parameter\. A user accesses your hosted UI from a link that contains either a `idp_identifier` or `identity_provider` parameter\. This user bypasses the sign\-in page and navigates directly to authenticate with your IdP\. For more information about naming SAML IdPs, see [Choosing SAML identity provider names](cognito-user-pools-managing-saml-idp-naming.md)\.

   Example URL:

   `https://your_Amazon_Cognito_userpool_domain/authorize?response_type=code&identity_provider=your-SAML-IdP-name&client_id=your-client-id&redirect_uri=https://your_application_redirect_url`

1. Amazon Cognito redirects the user session to your IdP with a SAML request\.

1. Your user might have received a session cookie from your IdP when they signed in to the dashboard\. Your IdP uses this cookie to validate the user silently and redirect them to the Amazon Cognito `idpresponse` endpoint with a SAML response\. If no active session exists, your IdP re\-authenticates the user before it posts the SAML response\.

1. Amazon Cognito validates the SAML response and creates or updates the user profile based on the SAML assertion\.

1. Amazon Cognito redirects the user to your internal app with an authorization code\. You configured your internal app URL as an authorized redirect URL for your app client\.

1. Your app exchanges the authorization code for Amazon Cognito tokens\. For more information, see [Token endpoint](token-endpoint.md)\.