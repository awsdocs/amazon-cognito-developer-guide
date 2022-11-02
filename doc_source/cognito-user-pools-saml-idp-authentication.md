# SAML user pool IdP authentication flow<a name="cognito-user-pools-saml-idp-authentication"></a>

You can integrate SAML\-based IdPs directly from your user pool\.

1. The app starts the sign\-up and sign\-in process by directing your user to the UI hosted by AWS\. A mobile app can use web view to show the pages hosted by AWS\.

1. Typically, your user pool determines the IdP for your user from that user's email address\.

   Alternatively, if your app gathered information before directing the user to your user pool, it can provide that information to Amazon Cognito through a query parameter\.

1. Your user is redirected to the IdP with a SAML request\.

1. The IdP authenticates the user if necessary\. If the IdP recognizes that the user has an active session, the IdP skips the authentication to provide a single sign\-in \(SSO\) experience\.

1. The IdP POSTs the SAML assertion to the Amazon Cognito service\.
**Note**  
Amazon Cognito cancels authentication requests that do not complete within 5 minutes, and redirects the user to the hosted UI\. The page displays a `Something went wrong` error message\.

1. After verifying the SAML assertion and collecting the user attributes \(claims\) from the assertion, Amazon Cognito internally creates or updates the user's profile in the user pool\. Amazon Cognito returns OIDC tokens to the app for the now signed\-in user\.

The following diagram shows the authentication flow for this process:

![\[Authentication flow diagram when Amazon Cognito uses SAML IdP with a user pool.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication flow diagram when Amazon Cognito uses SAML IdP with a user pool.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication flow diagram when Amazon Cognito uses SAML IdP with a user pool.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

When a user authenticates, the user pool returns ID, access, and refresh tokens\. The ID token is a standard OIDC token for identity management, while the access token is a standard [OAuth 2\.0](https://oauth.net/2/) token\. The ID and access tokens expire after one hour\. Your app can use a refresh token to get new tokens without having the user re\-authenticate\. 

As a developer, you can choose the expiration time for refresh tokens, which changes how frequently users need to reauthenticate\. If the user has authenticated through an external IdP as a federated user, your app uses the Amazon Cognito tokens with the refresh token to determine how long until the user reauthenticates, regardless of when the external IdP token expires\. The user pool automatically uses the refresh token to get new ID and access tokens when they expire\. If the refresh token has also expired, the server automatically initiates authentication through the pages in your app that AWS hosts\.

## Case sensitivity of SAML user names<a name="saml-nameid-case-sensitivity"></a>

When a federated user attempts to sign in, the SAML identity provider \(IdP\) passes a unique `NameId` from the IdP directory to Amazon Cognito in the user's SAML assertion\. Amazon Cognito identifies a SAML\-federated user by their `NameId` claim\. Regardless of the case sensitivity settings of your user pool, Amazon Cognito requires that a federated user from a SAML IdP pass a unique and case\-sensitive `NameId` claim\. If you map an attribute like `email` to `NameId`, and your user changes their email address, they can't sign in to your app\.

Map `NameId` in your SAML assertions from an IdP attribute that has values that don't change\.

For example, Carlos has a user profile in your case\-insensitive user pool from an Active Directory Federation Services \(ADFS\) SAML assertion that passed a `NameId` value of `Carlos@example.com`\. The next time Carlos attempts to sign in, your ADFS IdP passes a `NameId` value of `carlos@example.com`\. Because `NameId` must be an exact case match, the sign\-in doesn't succeed\.

If your users can't log in after their `NameID` changes, delete their user profiles from your user pool\. Amazon Cognito will create new user profiles the next time they sign in\.