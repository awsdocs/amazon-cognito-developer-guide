# Configuring Identity Providers for Your User Pool<a name="cognito-user-pools-identity-provider"></a>

**Note**  
The **Identity providers** tab appears only when you're editing an existing user pool\.

In the **Identity providers** tab, you can specify identity providers \(IdPs\) for your user pool\. For more information, see [Adding User Pool Sign\-in Through a Third Party](cognito-user-pools-identity-federation.md)\.

**Topics**
+ [Allowing Users to Sign in Using a Social Identity Provider](#cognito-user-pools-facebook-provider)
+ [Allowing Users to Sign in Using an OpenID Connect \(OIDC\) Identity Provider](#cognito-user-pools-oidc-providers)
+ [Allowing Users to Sign in Using SAML](#cognito-user-pools-saml-providers)

## Allowing Users to Sign in Using a Social Identity Provider<a name="cognito-user-pools-facebook-provider"></a>

You can use federation for Amazon Cognito user pools to integrate with social identity providers such as Facebook, Google, and Login with Amazon\.

To add a social identity provider, you first create a developer account with the identity provider\. Once you have your developer account, you register your app with the identity provider\. The identity provider creates an app ID and an app secret for your app, and you configure those values in your Amazon Cognito user pools\.

Here are links to help you get started with social identity providers:
+ [Google Identity Platform](https://developers.google.com/identity/)
+ [Facebook for Developers](https://developers.facebook.com/docs/facebook-login)
+ [Login with Amazon](https://developer.amazon.com/login-with-amazon)
+ [Sign in with Apple](https://developer.apple.com/sign-in-with-apple/)>

**To allow users to sign in using a social identity provider**

1. Choose a social identity provider such as **Facebook**, **Google**, **Login with Amazon**, or **SignInWithApple**\.

1. For the Facebook, Google or Amazon app ID and app secret, enter the app ID and app secret that you received when you created your Facebook, Google, or Login with Amazon client app\. For the Sign in with Apple services ID, team ID, key ID, and private key, enter the service ID you provided to Apple, and the team ID, key ID, and private key that you received when you created your Sign in with Apple client app\.

1. For **App secret**, enter the app secret that you received when you created your client app\.

1. For **Authorize scopes**, enter the names of the social identity provider scopes that you want to map to user pool attributes\. Scopes define which user attributes \(such as `name` and `email`\) you want to access with your app\. For Facebook, these should be separated by commas \(for example, `public_profile, email`\)\. For Google, Login with Amazon, and Sign in with Apple \(CLI\), they should be separated by spaces \(Google example: `profile email openid`\. Login with Amazon example: `profile postal_code`\. Sign in with Apple example: `name email`\)\. For Sign in with Apple \(console\), use the check boxes to select them\.

   The end\-user is asked to consent to providing these attributes to your app\. For more information about their scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign in with Apple\.

1. Choose **Enable Facebook**, **Enable Google**, **Enable Login with Amazon**, or **Enable Sign in with Apple**\.

For more information on Social IdPs see [Adding Social Identity Providers to a User Pool](cognito-user-pools-social-idp.md)\.

## Allowing Users to Sign in Using an OpenID Connect \(OIDC\) Identity Provider<a name="cognito-user-pools-oidc-providers"></a>

You can enable your users to sign in through an OIDC identity provider \(IdP\) such as Salesforce or Ping Identity\.

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose **OpenId Connect**\.

1. Type a unique name into **Provider name**\.

1. Type the OIDC IdP's client ID into **Client ID**\.

1. Type the OIDC IdP's client secret into **Client secret**\.

1. In the drop\-down list, choose the HTTP method \(either GET or POST\) that's used to fetch the details of the user from the **userinfo** endpoint into **Attributes request method**\.

1. Type the names of the scopes that you want to authorize\. Scopes define which user attributes \(such as `name` and `email`\) that you want to access with your application\. Scopes are separated by spaces, according to the [OAuth 2\.0](https://tools.ietf.org/html/rfc6749#section-3.3) specification\.

   Your app user is asked to consent to providing these attributes to your application\.

1. Type the URL of your IdP and choose **Run discovery**\.

   For example, Salesforce uses this URL:

   `https://login.salesforce.com` 
**Note**  
The URL should start with `https://`, and shouldn't end with a slash `/`\.

   1. If **Run discovery** isn't successful, then you need to provide the **Authorization endpoint**, **Token endpoint**, **Userinfo endpoint**, and **Jwks uri** \(the location of the [JSON Web Key](https://tools.ietf.org/html/rfc7517)\)\.

1. Choose **Create provider**\.

1. On the left navigation bar, choose **General settings**\.

1. Select your OIDC provider as one of the **Enabled Identity Providers**\.

1. Type a callback URL for the Amazon Cognito authorization server to call after users are authenticated\. This is the URL of the page where your user will be redirected after a successful sign\-in\.

   ```
   https://www.example.com
   ```

1. Under **Allowed OAuth Flows**, enable both the **Authorization code grant** and the **Implicit code grant**\.

   Unless you specifically want to exclude one, select the check boxes for all of the **Allowed OAuth scopes**\.

1. Choose **Save changes**\.

1. On the **Attribute mapping** tab on the left navigation bar, add mappings of OIDC claims to user pool attributes\.

   1. As a default, the OIDC claim **sub** is mapped to the user pool attribute **Username**\. You can map other OIDC [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims) to user pool attributes\. Type in the OIDC claim, and choose the corresponding user pool attribute from the drop\-down list\. For example, the claim **email** is often mapped to the user pool attribute **Email**\.

   1. In the drop\-down list, choose the destination user pool attribute\.

   1. Choose **Save changes**\.

   1. Choose **Go to summary**\.

For more information on OIDC IdPs see [Adding OIDC Identity Providers to a User Pool](cognito-user-pools-oidc-idp.md)\.

## Allowing Users to Sign in Using SAML<a name="cognito-user-pools-saml-providers"></a>

You can use federation for Amazon Cognito user pools to integrate with a SAML identity provider \(IdP\)\. You supply a metadata document, either by uploading the file or by entering a metadata document endpoint URL\. For information about obtaining metadata documents for third\-party SAML IdPs, see [Integrating Third\-Party SAML Identity Providers with Amazon Cognito User Pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.

**To allow users to sign in using SAML**

1. Choose **SAML** to display the SAML identity provider options\.

1. To upload a metadata document, choose **Select file**, or enter a metadata document endpoint URL\. The metadata document must be a valid XML file\.

1. Enter your SAML **Provider name**, for example, `"SAML_provider_1"`, and any **Identifiers** you want\. The provider name is required; the identifiers are optional\. For more information, see [Adding SAML Identity Providers to a User Pool](cognito-user-pools-saml-idp.md)\.

1. Select **Enable IdP sign out flow** when you want your user to be logged out from a SAML IdP when logging out from Amazon Cognito\.

   Enabling this flow sends a signed logout request to the SAML IdP when the [LOGOUT Endpoint](logout-endpoint.md) is called\.
**Note**  
If this option is selected and your SAML identity provider expects a signed logout request, you will also need to configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and logout your user from the Amazon Cognito session\.

1. Choose **Create provider**\.

1. To create additional providers, repeat the previous steps\.

**Note**  
If you see `InvalidParameterException` while creating a SAML identity provider with an HTTPS metadata endpoint URL, for example, "Error retrieving metadata from *<metadata endpoint>*," make sure that the metadata endpoint has SSL correctly set up and that there is a valid SSL certificate associated with it\.

**To set up the SAML IdP to add a signing certificate**
+ To get the certificate containing the public key which will be used by the identity provider to verify the signed logout request, choose **Show signing certificate** under **Active SAML Providers** on the **SAML** dialog under **Identity providers** on the **Federation** console page\.

For more information on SAML IdPs see [Adding SAML Identity Providers to a User Pool](cognito-user-pools-saml-idp.md)\.