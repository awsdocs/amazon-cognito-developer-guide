# Configuring identity providers for your user pool<a name="cognito-user-pools-identity-provider"></a>

**Note**  
The **Identity providers** tab appears only when you're editing an existing user pool\.

In the **Identity providers** tab, you can specify identity providers \(IdPs\) for your user pool\. For more information, see [Adding user pool sign\-in through a third party](cognito-user-pools-identity-federation.md)\.

**Topics**
+ [Set up user sign\-in with a social IdP](#cognito-user-pools-facebook-provider)
+ [Set up user sign\-in with an OIDC IdP](#cognito-user-pools-oidc-providers)
+ [Set up user sign\-in with a SAML IdP](#cognito-user-pools-saml-providers)

## Set up user sign\-in with a social IdP<a name="cognito-user-pools-facebook-provider"></a>

You can use federation to integrate Amazon Cognito user pools with social identity providers such as Facebook, Google, and Login with Amazon\.

To add a social identity provider, you first create a developer account with the identity provider\. After you have your developer account, register your app with the identity provider\. The identity provider creates an app ID and an app secret for your app, and you configure those values in your Amazon Cognito user pools\.

Here are links to help you get started with social identity providers:
+ [Google identity platform](https://developers.google.com/identity/)
+ [Facebook for developers](https://developers.facebook.com/docs/facebook-login)
+ [Login with Amazon](https://developer.amazon.com/login-with-amazon)
+ [Sign in with Apple](https://developer.apple.com/sign-in-with-apple/)>

------
#### [ Original console ]

**To integrate user sign\-in with a social IdP**

1. Choose a social identity provider such as **Facebook**, **Google**, **Login with Amazon**, or **Sign In with Apple**\.

1. Enter your social identity provider's information by completing one of the following steps, based on your choice of IdP:  
**Facebook, Google, and Login with Amazon**  
Enter the app ID and app secret that you received when you created your client app\.  
**Sign In with Apple**  
Enter the services ID that you provided to Apple, and the team ID, key ID, and private key you received when you created your app client\.

1. For **App secret**, enter the app secret that you received when you created your client app\.

1. For **Authorized scopes**, enter the names of the social identity provider scopes that you want to map to user pool attributes\. Scopes define which user attributes, such as name and email, you want to access with your app\. When entering scopes, use the following guidelines based on your choice of IdP:
   + **Facebook** — Separate scopes with commas\. For example:

     `public_profile, email`
   + **Google, Login with Amazon, and Sign In with Apple** — Separate scopes with spaces\. For example:
     + **Google:** `profile email openid`
     + **Login with Amazon:** `profile postal_code`
     + **Sign In with Apple:** `name email`
**Note**  
For Sign In with Apple \(console\), use the check boxes to choose scopes\.

   Your user will be asked to consent to providing these attributes to your app\. For more information about each social identity provider's scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign In with Apple\.

1. Choose **Enable Facebook**, **Enable Google**, **Enable Login with Amazon**, or **Enable Sign in with Apple**\.

------
#### [ New console ]

**To integrate user sign\-in with a social IdP**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. In the navigation pane, choose **User Pools**, and choose the user pool you want to edit\.

1. Choose the **Sign\-in experience** tab and locate **Federated sign\-in**\.

1. Choose **Add an identity provider**, or choose the **Facebook**, **Google**, **Amazon**, or **Apple** identity provider you have configured, locate **Identity provider information**, and choose **Edit**\. For more information about adding a social identity provider, see [Adding social identity providers to a user pool](cognito-user-pools-social-idp.md)\.

1. Enter your social identity provider's information by completing one of the following steps, based on your choice of IdP:  
**Facebook, Google, and Login with Amazon**  
Enter the app ID and app secret that you received when you created your client app\.  
**Sign In with Apple**  
Enter the service ID that you provided to Apple, and the team ID, key ID, and private key you received when you created your app client\.

1. For **Authorized scopes**, enter the names of the social identity provider scopes that you want to map to user pool attributes\. Scopes define which user attributes, such as name and email, that you want to access with your app\. When entering scopes, use the following guidelines based on your choice of IdP:
   + **Facebook** — Separate scopes with commas\. For example:

     `public_profile, email`
   + **Google, Login with Amazon, and Sign In with Apple** — Separate scopes with spaces\. For example:
     + **Google:** `profile email openid`
     + **Login with Amazon:** `profile postal_code`
     + **Sign In with Apple:** `name email`
**Note**  
For Sign In with Apple \(console\), use the check boxes to choose scopes\.

1. Choose **Save changes**\.

1. From the **App client integration** tab, choose one of the **App clients** in the list and then choose **Edit hosted UI settings**\. Add the new social identity provider to the app client under **Identity providers**\.

1. Choose **Save changes**\.

------

For more information on social IdPs, see [Adding social identity providers to a user pool](cognito-user-pools-social-idp.md)\.

## Set up user sign\-in with an OIDC IdP<a name="cognito-user-pools-oidc-providers"></a>

You can integrate user sign\-in with an OpenID Connect \(OIDC\) identity provider \(IdP\) such as Salesforce or Ping Identity\.

------
#### [ Original console ]

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose **OpenId Connect**\.

1. Enter a unique name into **Provider name**\.

1. Enter the OIDC IdP's client ID into **Client ID**\.

1. Enter the OIDC IdP's client secret into **Client secret**\.

1. In the drop\-down list, select the HTTP method \(either GET or POST\) that Amazon Cognito uses to fetch user details from the **userInfo** endpoint into **Attributes request method**\.

1. Enter the names of the scopes that you want to authorize\. Scopes define which user attributes \(such as `name` and `email`\) that you want to access with your application\. Scopes are separated by spaces, according to the [OAuth 2\.0](https://tools.ietf.org/html/rfc6749#section-3.3) specification\.

   Your app user is asked to consent to providing these attributes to your application\.

1. Enter the URL of your IdP and choose **Run discovery**\. For example, Salesforce uses this URL:

   `https://login.salesforce.com` 
**Note**  
The URL should start with `https://`, and shouldn't end with a slash `/`\.

   1. If **Run discovery** isn't successful, then you must provide the **Authorization endpoint**, **Token endpoint**, **Userinfo endpoint**, and **Jwks uri** \(the location of the [JSON web key](https://tools.ietf.org/html/rfc7517)\)\.

1. Choose **Create provider**\.

1. On the left navigation bar, choose **App client settings**\.

1. Select your OIDC provider as one of the **Enabled Identity Providers**\.

1. Enter a callback URL for the Amazon Cognito authorization server to call after users are authenticated\. This is the URL of the page where Amazon Cognito directs your user after a successful sign\-in\.

   ```
   https://www.example.com
   ```

1. Under **Allowed OAuth Flows**, activate both the **Authorization code grant** and the **Implicit code grant**\.

   Unless you specifically want to exclude one, select the check boxes for all of the **Allowed OAuth scopes**\.

1. Choose **Save changes**\.

1. On the **Attribute mapping** tab on the left navigation bar, add mappings of OIDC claims to user pool attributes\.

   1. As a default, the OIDC claim **sub** is mapped to the user pool attribute **Username**\. You can map other OIDC [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims) to user pool attributes\. Enter the OIDC claim, and select the corresponding user pool attribute from the drop\-down list\. For example, the claim **email** is typically mapped to the user pool attribute **Email**\.

   1. In the drop\-down list, select the destination user pool attribute\.

   1. Choose **Save changes**\.

   1. Choose **Go to summary**\.

------
#### [ New console ]

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools** from the navigation menu\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and select **Add an identity provider**\.

1. Choose an **OpenID Connect** identity provider\.

1. Enter a unique name into **Provider name**\.

1. Enter the client ID that you received from your provider into **Client ID**\.

1. Enter the client secret that you received from your provider into **Client secret**\.

1. Enter **Authorized scopes** for this provider\. Scopes define which groups of user attributes \(such as `name` and `email`\) that your application will request from your provider\. Scopes must be separated by spaces, following the [OAuth 2\.0](https://tools.ietf.org/html/rfc6749#section-3.3) specification\.

   Your user must consent to provide these attributes to your application\.

1. Choose an **Attribute request method** to provide Amazon Cognito with the HTTP method \(either GET or POST\) that Amazon Cognito uses to fetch the details of the user from the **userInfo** endpoint operated by your provider\.

1. Choose a **Setup method** to retrieve OpenID Connect endpoints either by **Auto fill through issuer URL** or **Manual input**\. Use **Auto fill through issuer URL** when your provider has a public `.well-known/openid-configuration` endpoint where Amazon Cognito can retrieve the URLs of the `authorization`, `token`, `userInfo`, and `jwks_uri` endpoints\.

1. Enter the issuer URL or `authorization`, `token`, `userInfo`, and `jwks_uri` endpoint URLs from your IdP\.
**Note**  
You can use only port numbers 443 and 80 with discovery, auto\-filled, and manually entered URLs\. User logins fail if your OIDC provider uses any nonstandard TCP ports\.  
The issuer URL must start with `https://`, and must not end with a `/` character\. For example, Salesforce uses this URL:  
`https://login.salesforce.com`   
The `openid-configuration` document associated with your issuer URL must provide HTTPS URLs for the following values: `authorization_endpoint`, `token_endpoint`, `userinfo_endpoint`, and `jwks_uri`\. Similarly, when you choose **Manual input**, you can only enter HTTPS URLs\.

1. The OIDC claim **sub** is mapped to the user pool attribute **Username** by default\. You can map other OIDC [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims) to user pool attributes\. Enter the OIDC claim, and select the corresponding user pool attribute from the drop\-down list\. For example, the claim **email** is often mapped to the user pool attribute **Email**\.

1. Map additional attributes from your identity provider to your user pool\. For more information, see [Specifying Identity Provider attribute mappings for your user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-specifying-attribute-mapping.html)\.

1. Choose **Create**\.

1. From the **App client integration** tab, select one of the **App clients** in the list and **Edit hosted UI settings**\. Add the new OIDC identity provider to the app client under **Identity providers**\.

1. Choose **Save changes**\.

------

For more information on OIDC IdPs, see [Adding OIDC identity providers to a user pool](cognito-user-pools-oidc-idp.md)\.

## Set up user sign\-in with a SAML IdP<a name="cognito-user-pools-saml-providers"></a>

You can use federation for Amazon Cognito user pools to integrate with a SAML identity provider \(IdP\)\. You supply a metadata document, either by uploading the file or by entering a metadata document endpoint URL\. For information about obtaining metadata documents for third\-party SAML IdPs, see [Integrating third\-party SAML identity providers with Amazon Cognito user pools](cognito-user-pools-integrating-3rd-party-saml-providers.md)\.

------
#### [ Original console ]

**To integrate user sign\-in with a SAML IdP**

1. Choose **SAML** to display the SAML identity provider options\.

1. To upload a metadata document, choose **Select file**, or enter a metadata document endpoint URL\. The metadata document must be a valid XML file\.

1. Enter your SAML **Provider name**, for example, `"SAML_provider_1"`, and any **Identifiers** you want\. The provider name is required; the identifiers are optional\. For more information, see [Adding SAML identity providers to a user pool](cognito-user-pools-saml-idp.md)\.

1. Select **Enable IdP sign out flow** when you want your user to be logged out from a SAML IdP when logging out from Amazon Cognito\.

   Enabling this flow sends a signed logout request to the SAML IdP when the [LOGOUT endpoint](logout-endpoint.md) is called\.
**Note**  
If you select this option and your SAML IdP expects a signed logout request, you also must configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and logout your user from the Amazon Cognito session\.

1. Choose **Create provider**\.

1. To create additional providers, repeat the previous steps\.

------
#### [ New console ]

**To configure a SAML 2\.0 identity provider in your user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and select **Add an identity provider**\.

1. Choose a **SAML** identity provider\.

1. Enter **Identifiers** separated by commas\. An identifier directs Amazon Cognito to check the user sign\-in email address, and then direct the user to the provider that corresponds to their domain\.

1. Choose **Add sign\-out flow** if you want Amazon Cognito to send signed sign\-out requests to your provider when a user logs out\. Configure your SAML 2\.0 identity provider to send sign\-out responses to the `https://<your Amazon Cognito domain>/saml2/logout` endpoint that Amazon Cognito creates when you configure the hosted UI\. The `saml2/logout` endpoint uses POST binding\.
**Note**  
If you select this option and your SAML identity provider expects a signed logout request, you also must configure the signing certificate provided by Amazon Cognito with your SAML IdP\.   
The SAML IdP will process the signed logout request and logout your user from the Amazon Cognito session\.

1. Choose a **Metadata document source**\. If your identity provider offers SAML metadata at a public URL, you can choose **Metadata document URL**and enter that public URL\. Otherwise, choose **Upload metadata document** and select a metadata file you downloaded from your provider earlier\.
**Note**  
If your provider has a public endpoint, we recommend that you enter a metadata document URL, rather than uploading a file\. If you use the URL, Amazon Cognito refreshes metadata automatically\. Typically, metadata refresh happens every 6 hours or before the metadata expires, whichever is earlier\.

1. **Map attributes between your SAML provider and your app** to map SAML provider attributes to the user profile in your user pool\. Include your user pool required attributes in your attribute map\. 

   For example, when you choose **User pool attribute** `email`, enter the SAML attribute name as it appears in the SAML assertion from your identity provider\. Your identity provider might offer sample SAML assertions for reference\. Some identity providers use simple names, such as `email`, while others use URL\-formatted attribute names similar to:

   ```
   http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress
   ```

1. Choose **Create**\.

------

**Note**  
If you see `InvalidParameterException` while creating a SAML IdP with an HTTPS metadata endpoint URL, make sure that the metadata endpoint has SSL correctly set up and that there is a valid SSL certificate associated with it\. One example of such an exception would be "Error retrieving metadata from *<metadata endpoint>*"\.

**To set up the SAML IdP to add a signing certificate**
+ To get the certificate containing the public key that the IdP uses to verify the signed logout request, choose **Show signing certificate** under **Active SAML Providers** on the **SAML** dialog under **Identity providers** on the **Federation** console page\.

For more information on SAML IdPs see [Adding SAML identity providers to a user pool](cognito-user-pools-saml-idp.md)\.
