# SAML User Pool IdP Authentication Flow<a name="cognito-user-pools-saml-idp-authentication"></a>

You can integrate SAML\-based IdPs directly from your user pool\.

1. The app starts the sign\-up and sign\-in process by directing your user to the UI hosted by AWS\. A mobile app can use web view to show the pages hosted by AWS\.

1. Typically your user pool determines the identity provider for your user from that user's email address\.

   Alternatively, if your app gathered information before directing the user to your user pool, it can provide that information to Amazon Cognito through a query parameter\.

1. Your user is redirected to the identity provider\.

1. The IdP authenticates the user if necessary\. If the IdP recognizes that the user has an active session, the IdP skips the authentication to provide a single sign\-in \(SSO\) experience\.

1. The IdP POSTs the SAML assertion to the Amazon Cognito service\.

1. After verifying the SAML assertion and collecting the user attributes \(claims\) from the assertion, Amazon Cognito internally creates or updates the user's profile in the userpool\. Amazon Cognito returns OIDC tokens to the app for the now signed\-in user\.

The following diagram shows the authentication flow for this process:

![\[Authentication flow diagram for using SAML IdP with a user pool.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication flow diagram for using SAML IdP with a user pool.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication flow diagram for using SAML IdP with a user pool.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Requests that are not completed within 5 minutes will be cancelled, redirected to the login page, and then display a `Something went wrong` error message\.

When a user authenticates, the user pool returns ID, access, and refresh tokens\. The ID token is a standard OIDC token for identity management, and the access token is a standard [OAuth 2\.0](https://oauth.net/2/) token\. The ID and access tokens expire after one hour, but your app can use the refresh token to get new tokens without having the user re\-authenticate\. As a developer, you can choose the expiration time of refresh tokens, and therefore how frequently users need to reauthenticate\. If the user has authenticated through an external IdP \(i\.e\., they are a federated user\), your app still uses the Amazon Cognito tokens with the refresh token to determine how long until the user reauthenticates, regardless of when the external IdP's token expires\. The user pool automatically uses the refresh token to get new ID and access tokens when they expire\. If the refresh token has also expired, the server automatically initiates authentication through the pages in your app that are hosted by AWS\.