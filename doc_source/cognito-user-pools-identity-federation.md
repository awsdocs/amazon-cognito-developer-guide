# Adding user pool sign\-in through a third party<a name="cognito-user-pools-identity-federation"></a>

Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\. With the built\-in hosted web UI, Amazon Cognito provides token handling and management for authenticated users from all IdPs\. This way, your backend systems can standardize on one set of user pool tokens\.

## How federated sign\-in works in Amazon Cognito user pools<a name="cognito-user-pools-identity-federation-how-it-works"></a>

Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Amazon Cognito is a user directory and an OAuth 2\.0 identity provider \(IdP\)\. When you sign in *native users* to the Amazon Cognito directory, your user pool is an IdP to your app\. Native users are those who sign up or who your administrator creates directly in your user pool\.

When you connect Amazon Cognito to social, SAML, or OpenID Connect \(OIDC\) IdPs, your user pool acts as a bridge between multiple service providers and your app\. To your IdP, Amazon Cognito is a service provider \(SP\)\. Your IdPs pass an OIDC ID token or a SAML assertion to Amazon Cognito\. Amazon Cognito reads the claims about your user in the token or assertion and maps those claims to a new user profile in your user pool directory\.

Amazon Cognito then creates a user profile for your federated user in its own directory\. Amazon Cognito adds attributes to your user based on the claims from your IdP and, in the case of OIDC and social identity providers, an IdP\-operated public `userinfo` endpoint\. Your user's attributes change in your user pool when a mapped IdP attribute changes\. You can also add more attributes independent of those from the IdP\.

After Amazon Cognito creates a profile for your federated user, it changes its function and presents itself as the IdP to your app, which is now the SP\. Amazon Cognito is a combination OIDC and OAuth 2\.0 IdP\. It generates access tokens, ID tokens, and refresh tokens\. For more information about tokens, see [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

You must design an app that integrates with Amazon Cognito to authenticate and authorize your users, whether federated or native\.<a name="cognito-user-pools-identity-federation-how-it-works-app-responsibilities"></a>

## The responsibilities of an app as an Amazon Cognito SP<a name="cognito-user-pools-identity-federation-how-it-works-app-responsibilities"></a>

**Verify and process the information in the tokens**  
In most scenarios, Amazon Cognito redirects your authenticated user to an app URL that it appends with an authorization code\. Your app [exchanges the code](https://docs.aws.amazon.com/cognito/latest/developerguide/token-endpoint.html) for access, ID, and refresh tokens\. Then, it must [check the validity of the tokens](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-verifying-a-jwt.html) and serve information to your user based on the claims in the tokens\.

**Respond to authentication events with Amazon Cognito API requests**  
Your app must integrate with the [Amazon Cognito user pools API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html) and the [ authentication API endpoints](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html)\. The authentication API signs your user in and out, and manages tokens\. The user pools API has a variety of operations that manage your user pool, your users, and the security of your authentication environment\. Your app must know what to do next when it receives a response from Amazon Cognito\.<a name="cognito-user-pools-identity-federation-how-it-works-considerations"></a>

# Things to know about Amazon Cognito user pools third\-party sign\-in<a name="cognito-user-pools-identity-federation-how-it-works-considerations"></a>
+ If you want your users to sign in with federated providers, you must choose a domain\. This sets up the Amazon Cognito hosted UI and [hosted UI and OIDC endpoints](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html)\. For more information, see [Using your own domain for the hosted UI](cognito-user-pools-add-custom-domain.md)\.
+ You can't sign in federated users with API operations like [ InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) and [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html)\. Federated users can only sign in with the [Login endpoint](login-endpoint.md) or the [Authorize endpoint](authorization-endpoint.md)\.
+ The [Authorize endpoint](authorization-endpoint.md) is a *redirection* endpoint\. If you provide an `idp_identifier` or `identity_provider` parameter in your request, it redirects silently to your IdP, bypassing the hosted UI\. Otherwise, it redirects to the hosted UI [Login endpoint](login-endpoint.md)\. For an example, see [Bookmark Amazon Cognito apps in an enterprise dashboard](cognito-user-pools-SAML-session-initiation.md#bookmark-applications-in-idp-portal)\.
+ When the hosted UI redirects a session to a federated IdP, Amazon Cognito includes the `user-agent` header `Amazon/Cognito` in the request\.
+ Amazon Cognito derives the `username` attribute for a federated user profile from a combination of a fixed identifier and the name of your IdP\. To generate a user name that matches your custom requirements, create a mapping to the `preferred_username` attribute\. For more information, see [Things to know about mappings](cognito-user-pools-specifying-attribute-mapping.md#cognito-user-pools-specifying-attribute-mapping-requirements)\.

  Example: `MyIDP_bob@example.com`
+ Amazon Cognito records information about your federated user's identity to an attribute, and a claim in the ID token, called `identities`\. This claim contains your user's provider and their unique ID from the provider\. You can't change the `identities` attribute in a user profile directly\. For more information about how to link a federated user, see [Linking federated users to an existing user profile](cognito-user-pools-identity-federation-consolidate-users.md)\.
+ When you update your IdP in an [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateIdentityProvider.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateIdentityProvider.html) API request, your changes can take up to a minute to appear in the hosted UI\.
+ Amazon Cognito supports up to 20 HTTP redirects between itself and your IdP\.

**Topics**
+ [How federated sign\-in works in Amazon Cognito user pools](#cognito-user-pools-identity-federation-how-it-works)
+ [Adding social identity providers to a user pool](cognito-user-pools-social-idp.md)
+ [Adding SAML identity providers to a user pool](cognito-user-pools-saml-idp.md)
+ [Adding OIDC identity providers to a user pool](cognito-user-pools-oidc-idp.md)
+ [Specifying identity provider attribute mappings for your user pool](cognito-user-pools-specifying-attribute-mapping.md)
+ [Linking federated users to an existing user profile](cognito-user-pools-identity-federation-consolidate-users.md)