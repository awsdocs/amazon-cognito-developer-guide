# Adding OIDC identity providers to a user pool<a name="cognito-user-pools-oidc-idp"></a>

You can enable your users who already have accounts with [OpenID Connect \(OIDC\)](http://openid.net/specs/openid-connect-core-1_0.html) identity providers \(IdPs\) to skip the sign\-up step and sign in to your application using an existing account\. With the built\-in hosted web UI, Amazon Cognito provides token handling and management for all authenticated users\. This way, your backend systems can standardize on one set of user pool tokens\.

![\[Authentication overview with an OIDC IdP\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with an OIDC IdP\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with an OIDC IdP\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

You can add an OIDC IdP to your user pool in the AWS Management Console, through the AWS CLI, or with the user pool API method [CreateIdentityProvider](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateIdentityProvider.html)\.

**Topics**
+ [Prerequisites](#cognito-user-pools-oidc-idp-prerequisites)
+ [Step 1: Register with an OIDC IdP](#cognito-user-pools-oidc-idp-step-1)
+ [Step 2: Add an OIDC IdP to your user pool](#cognito-user-pools-oidc-idp-step-2)
+ [Step 3: Test your OIDC IdP configuration](#cognito-user-pools-oidc-idp-step-3)
+ [OIDC user pool IdP authentication flow](cognito-user-pools-oidc-flow.md)

## Prerequisites<a name="cognito-user-pools-oidc-idp-prerequisites"></a>

Before you begin, you need the following:
+ A user pool with an app client and a user pool domain\. For more information, see [Create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.
+ An OIDC IdP with the following configuration: 
  + Supports `client_secret_post` client authentication\. Amazon Cognito doesn't check the `token_endpoint_auth_methods_supported` claim at the OIDC discovery endpoint for your IdP\. Amazon Cognito doesn't support `client_secret_basic` client authentication\. For more information on client authentication, see [Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) in the OpenID Connect documentation\.
  + Only uses HTTPS for OIDC endpoints such as `openid_configuration`, `userInfo`, and `jwks_uri`\.
  + Only uses TCP ports 80 and 443 for OIDC endpoints\.
  + Only signs ID tokens with HMAC\-SHA, ECDSA, or RSA algorithms\.

## Step 1: Register with an OIDC IdP<a name="cognito-user-pools-oidc-idp-step-1"></a>

Before you create an OIDC IdP with Amazon Cognito, you must register your application with the OIDC IdP to receive a client ID and client secret\.

**To register with an OIDC IdP**

1. Create a developer account with the OIDC IdP\.  
**Links to OIDC IdPs**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-oidc-idp.html)

1. Register your user pool domain URL with the `/oauth2/idpresponse` endpoint with your OIDC IdP\. This ensures that the OIDC IdP later accepts it from Amazon Cognito when it authenticates users\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Register your callback URL with your Amazon Cognito user pool\. This is the URL of the page where Amazon Cognito redirects your user after a successful authentication\.

   ```
   https://www.example.com
   ```

1. Select your [scopes](https://openid.net/specs/openid-connect-basic-1_0.html#Scopes)\. The scope **openid** is required\. The **email** scope is needed to grant access to the **email** and **email\_verified** [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims)\.

1. The OIDC IdP provides you with a client ID and a client secret\. You'll use them when you set up an OIDC IdP in your user pool\.

**Example: Use Salesforce as an OIDC IdP with your user pool**

 You use an OIDC IdP when you want to establish trust between an OIDC\-compatible IdP such as Salesforce and your user pool\.

1. [Create an account](https://developer.salesforce.com/signup) on the Salesforce Developers website\.

1. [Sign in](https://developer.salesforce.com) through your developer account that you set up in the previous step\.

1. From your Salesforce page, do one of the following:
   +  If you’re using Lightning Experience, choose the setup gear icon, then choose **Setup Home**\.
   +  If you’re using Salesforce Classic and you see **Setup** in the user interface header, choose it\.
   +  If you’re using Salesforce Classic and you don’t see **Setup** in the header, choose your name from the top navigation bar, and choose **Setup** from the drop\-down list\.

1. On the left navigation bar, choose **Company Settings**\. 

1. On the navigation bar, choose **Domain**, enter a domain, and choose **Create**\. 

1. On the left navigation bar, under **Platform Tools**, choose **Apps**\. 

1. Choose **App Manager**\.

1. 

   1. Choose **New connected app**\.

   1. Complete the required fields\.

      Under **Start URL**, enter a URL at the `/authorize` endpoint for the user pool domain that signs in with your Salesforce IdP\. When your users access your connected app, Salesforce directs them to this URL to complete sign\-in\. Then Salesforce redirects the users to the callback URL that you have associated with your app client\.

      ```
      https://<your_user_pool_domain>/authorize?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com&identity_provider=CorpSalesforce
      ```

   1. Enable **OAuth settings** and enter the URL of the `/oauth2/idpresponse` endpoint for your user pool domain in **Callback URL**\. This is the URL where Salesforce issues the authorization code that Amazon Cognito exchanges for an OAuth token\.

      ```
      https://<your_user_pool_domain>/oauth2/idpresponse
      ```

1. Select your [scopes](https://openid.net/specs/openid-connect-basic-1_0.html#Scopes)\. You must include the scope **openid**\. To grant access to the **email** and **email\_verified** [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims), add the **email** scope\. Separate scopes by spaces\.

1. Choose **Create**\.

   In Salesforce, the client ID is called a **Consumer Key**, and the client secret is a **Consumer Secret**\. Note your client ID and client secret\. You will use them in the next section\.

## Step 2: Add an OIDC IdP to your user pool<a name="cognito-user-pools-oidc-idp-step-2"></a>

In this section, you configure your user pool to process OIDC\-based authentication requests from an OIDC IdP\.

### To add an OIDC IdP \(Amazon Cognito console\)<a name="cognito-user-pools-oidc-idp-step-2-console"></a>

------
#### [ Original console ]

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose **OpenId Connect**\.

1. Enter a unique name into **Provider name**\.

1. Enter the OIDC IdP client ID from the previous section into **Client ID**\.

1. Enter the client secret from the previous section into **Client secret**\.

1. In the drop\-down list, select the HTTP method \(either GET or POST\) used to fetch the details of the user from the **userinfo** endpoint into **Attributes request method**\.

1. Enter the names of the scopes that you want to authorize\. Scopes define which user attributes \(such as `name` and `email`\) that you want to access with your application\. Scopes are separated by spaces, according to the [OAuth 2\.0](https://tools.ietf.org/html/rfc6749#section-3.3) specification\.

   Your app user is asked to consent to providing these attributes to your application\.

1. Enter the URL of your IdP and choose **Run discovery**\.
**Note**  
The URL should start with `https://`, and shouldn't end with a slash `/`\. Only port numbers 443 and 80 can be used with this URL\. For example, Salesforce uses this URL:  
`https://login.salesforce.com` 

   1. If **Run discovery** isn't successful, then you need to provide the **Authorization endpoint**, **Token endpoint**, **Userinfo endpoint**, and **Jwks uri** \(the location of the [JSON Web Key](https://tools.ietf.org/html/rfc7517)\)\.
**Note**  
If provider uses discovery for federated login, the discovery document must use HTTPS for the following values: `authorization_endpoint`, `token_endpoint`, `userinfo_endpoint`, and `jwks_uri`\. Otherwise the login will fail\.

1. Choose **Create provider**\.

1. On the left navigation bar, choose **App client settings**\.

1. Select the OIDC provider that you set up in the previous step as one of the **Enabled Identity Providers**\.

1. Enter a callback URL for the Amazon Cognito authorization server to call after users are authenticated\. This is the URL of the page where your user will be redirected after a successful authentication\.

   ```
   https://www.example.com
   ```

1. Under **Allowed OAuth Flows**, enable both the **Authorization code grant** and the **Implicit code grant**\.

   Unless you specifically want to exclude one, select the check boxes for all of the **Allowed OAuth scopes**\.

1. Choose **Save changes**\.

1. On the **Attribute mapping** tab on the left navigation bar, add mappings of OIDC claims to user pool attributes\.

   1. As a default, the OIDC claim **sub** is mapped to the user pool attribute **Username**\. You can map other OIDC [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims) to user pool attributes\. Enter the OIDC claim, and choose the corresponding user pool attribute from the drop\-down list\. For example, the claim **email** is often mapped to the user pool attribute **Email**\.

   1. In the drop\-down list, choose the destination user pool attribute\.

   1. Choose **Save changes**\.

   1. Choose **Go to summary**\.

------
#### [ New console ]

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools** from the navigation menu\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and select **Add an identity provider**\.

1. Choose an **OpenID Connect** IdP\.

1. Enter a unique name into **Provider name**\.

1. Enter the client ID you received from your provider into **Client ID**\.

1. Enter the client secret you received from your provider into **Client secret**\.

1. Enter **Authorized scopes** for this provider\. Scopes define which groups of user attributes \(such as `name` and `email`\) that your application will request from your provider\. Scopes must be separated by spaces, following the [OAuth 2\.0](https://tools.ietf.org/html/rfc6749#section-3.3) specification\.

   Your user will be asked to consent to providing these attributes to your application\.

1. Choose an **Attribute request method** to provide Amazon Cognito with the HTTP method \(either GET or POST\) that it must use to fetch the details of the user from the **userInfo** endpoint operated by your provider\.

1. Choose a **Setup method** to retrieve OpenID Connect endpoints either by **Auto fill through issuer URL** or **Manual input**\. Use **Auto fill through issuer URL** when your provider has a public `.well-known/openid-configuration` endpoint where Amazon Cognito can retrieve the URLs of the `authorization`, `token`, `userInfo`, and `jwks_uri` endpoints\.

1. Enter the issuer URL or `authorization`, `token`, `userInfo`, and `jwks_uri` endpoint URLs from your IdP\.
**Note**  
The URL should start with `https://`, and shouldn't end with a slash `/`\. Only port numbers 443 and 80 can be used with this URL\. For example, Salesforce uses this URL:  
`https://login.salesforce.com`   
If you choose auto fill, the discovery document must use HTTPS for the following values: `authorization_endpoint`, `token_endpoint`, `userinfo_endpoint`, and `jwks_uri`\. Otherwise the login will fail\.

1. The OIDC claim **sub** is mapped to the user pool attribute **Username** by default\. You can map other OIDC [claims](https://openid.net/specs/openid-connect-basic-1_0.html#StandardClaims) to user pool attributes\. Enter the OIDC claim, and choose the corresponding user pool attribute from the drop\-down list\. For example, the claim **email** is often mapped to the user pool attribute **Email**\.

1. Map attributes from your IdP to your user pool\. For more information, see [Specifying Identity Provider Attribute Mappings for Your User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-specifying-attribute-mapping.html)\.

1. Choose **Create**\.

1. From the **App client integration** tab, choose one of the **App clients** in the list and **Edit hosted UI settings**\. Add the new OIDC IdP to the app client under **Identity providers**\.

1. Choose **Save changes**\.

------

### To add an OIDC IdP \(AWS CLI\)<a name="cognito-user-pools-oidc-idp-step-2-cli"></a>
+ See the parameter descriptions for the [CreateIdentityProvider](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateIdentityProvider.html) API method\.

  ```
                          
  aws cognito-idp create-identity-provider
  --user-pool-id string
  --provider-name string
  --provider-type OIDC
  --provider-details map
  
  
  --attribute-mapping string
  --idp-identifiers (list)
  --cli-input-json string
  --generate-cli-skeleton string
  ```

  Use this map of provider details:

  ```
                      
                          
  { 
    "client_id": "string",
    "client_secret": "string",
    "authorize_scopes": "string",
    "attributes_request_method": "string",
    "oidc_issuer": "string",
  
    "authorize_url": "string",
    "token_url": "string",
    "attributes_url": "string",
    "jwks_uri": "string"
  }
  ```

## Step 3: Test your OIDC IdP configuration<a name="cognito-user-pools-oidc-idp-step-3"></a>

You can create the authorization URL by using the elements from the previous two sections, and using them to test your OIDC IdP configuration\.

```
https://<your_user_pool_domain>/oauth2/authorize?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com
```

You can find your domain on the user pool **Domain name** console page\. The client\_id is on the **General settings** page\. Use your callback URL for the **redirect\_uri** parameter\. This is the URL of the page where your user will be redirected after a successful authentication\.