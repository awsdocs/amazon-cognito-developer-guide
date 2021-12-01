# Step 3\. Add social sign\-in to a user pool \(optional\)<a name="cognito-user-pools-configuring-federation-with-social-idp"></a>

You can enable your app users to sign in through a social identity provider \(IdP\) such as Facebook, Google, Amazon, and Apple\. Whether your users sign in directly or through a third party, all users have a profile in the user pool\. Skip this step if you don't want to add sign in through a social sign\-in identity provider\. 

## Step 1: Register with a social IdP<a name="cognito-user-pools-social-step-1"></a>

Before you create a social IdP with Amazon Cognito, you must register your application with the social IdP to receive a client ID and client secret\.

### To register an app with Facebook<a name="register-an-app-with-facebook"></a>

1. Create a [developer account with Facebook](https://developers.facebook.com/docs/facebook-login)\.

1. [Sign in](https://developers.facebook.com/) with your Facebook credentials\.

1. From the **My Apps** menu, choose **Create New App**\.

1. Enter a name for your Facebook app and choose **Create App ID**\.

1. On the left navigation bar, choose **Settings**, and then choose **Basic**\.

1. Note the **App ID** and the **App Secret**\. You will use them in the next section\.

1. Choose **\+ Add Platform** from the bottom of the page\.

1. Choose **Website**\.

1. Under **Website**, enter a sign\-in URL for your app client endpoint into **Site URL**\. Your sign\-in URL should be in the following format:

   ```
   https://your_user_pool_domain/login?response_type=code&client_id=your_app_client_id&redirect_uri=your_callback_url
   ```

1. Choose **Save changes**\.

1. For **App Domains**, enter your user pool domain\.

   ```
   https://your_user_pool_domain
   ```

1. Choose **Save changes**\.

1. From the navigation bar, choose **Products**, and then **Set up** from **Facebook Login**\.

1. From the navigation bar, choose **Facebook Login** and then **Settings**\.

   Enter your redirect URL into **Valid OAuth Redirect URIs**\. The redirect URL will consist of your user pool domain with the `/oauth2/idpresponse` endpoint\.

   ```
   https://your_user_pool_domain/oauth2/idpresponse
   ```

1. Choose **Save changes**\.

### To register an app with Amazon<a name="register-an-app-with-amazon"></a>

1. Create a [developer account with Amazon](https://developer.amazon.com/login-with-amazon)\.

1. [Sign in](https://developer.amazon.com/lwa/sp/overview.html) with your Amazon credentials\.

1. You need to create an Amazon security profile to receive the Amazon client ID and client secret\.

   Choose **Apps and Services** from navigation bar at the top of the page and then choose **Login with Amazon**\.

1. Choose **Create a Security Profile**\.

1. Enter a **Security Profile Name**, a **Security Profile Description**, and a **Consent Privacy Notice URL**\.

1. Choose **Save**\.

1. Choose **Client ID** and **Client Secret** to show the client ID and secret\. You will use them in the next section\.

1. Hover over the gear icon and choose **Web Settings**, and then choose **Edit**\.

1. Enter your user pool domain into **Allowed Origins**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Enter your user pool domain with the `/oauth2/idpresponse` endpoint into **Allowed Return URLs**\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Save**\.

### To register an app with Google<a name="register-an-app-with-google"></a>

1. Create a [developer account with Google](https://developers.google.com/identity)\.

1. [Sign in](https://developers.google.com/identity/sign-in/web/sign-in) with your Google credentials\.

1. Choose **CONFIGURE A PROJECT**\.

1. Enter a project name, and then choose **NEXT**\.

1. Enter your product name, and then choose **NEXT**\.

1. Select **Web browser** from the **Where are you calling from?** drop\-down list\.

1. Enter your user pool domain into the **Authorized JavaScript origins** field\.

   ```
   https://<your-user-pool-domain>
   ```

1. Choose **CREATE**\. You will not use the **Client ID** and **Client Secret** from this step\.

1. Choose **DONE**\.

1. [Sign in](https://console.developers.google.com) to the Google Console\.

1. On the left navigation bar, choose **Credentials**\.

1. Create your OAuth 2\.0 credentials by choosing **OAuth client ID** from the **Create credentials** drop\-down list\.

1. Choose **Web application**\.

1. Enter your user pool domain into the **Authorized JavaScript origins** field\.

   ```
   https://<your-user-pool-domain>
   ```

1. Enter your user pool domain with the `/oauth2/idpresponse` endpoint into the **Authorized Redirect URIs** field\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Create** twice\.

1. Note the **OAuth client ID** and **client secret**\. You will need them for the next section\.

1. Choose **OK**\.

### To register an app with Apple<a name="register-an-app-with-apple"></a>

1. Create a [developer account with Apple](https://developer.apple.com/programs/enroll/)\.

1. [Sign in](https://developer.apple.com/account/#/welcome) with your Apple credentials\.

1. On the left navigation bar, choose **Certificates, IDs & Profiles**\.

1. On the left navigation bar, choose **Identifiers**\.

1. On the **Identifiers** page, choose the **\+** icon\.

1. On the **Register a New Identifier** page, choose **App IDs**, and then choose **Continue**\.

1. On the **Register an App ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **App ID Prefix**, type an identifier\. Make a note of the value under **App ID Prefix**\. You will use this value after you choose Apple as your identity provider in [Step 2: Add a social IdP to your user pool](cognito-user-pools-social-idp.md#cognito-user-pools-social-idp-step-2)\.

   1. Under **Capabilities**, choose **Sign In with Apple**, and then choose **Edit**\.

   1. On the **Sign in with Apple: App ID Configuration** page, select the appropriate setting for you app, and then choose **Save**\.

   1. Choose **Continue**\.

1. On the **Confirm your App ID** page, choose **Register**\.

1. On the **Identifiers** page, hover over **App IDs** on the right side of the page, choose **Services IDs**, and then choose the **\+** icon\.

1. On the **Register a New Identifier** page, choose **Services IDs**, and then choose **Continue**\.

1. On the **Register a Services ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **Identifier**, type an identifier\. Make a note of this Services ID as you will need this value after you choose Apple as your identity provider in [Step 2: Add a social IdP to your user pool](cognito-user-pools-social-idp.md#cognito-user-pools-social-idp-step-2)\.

   1. Select **Sign In with Apple**, and then choose **Configure**\.

   1. On the **Web Authentication Configuration** page, choose a **Primary App ID**\. Under **Web Domain**, type your user pool domain\. Under **Return URLs**, type your user pool domain and include the `/oauth2/idpresponse` endpoint\. For example:

      ```
      https://<your-user-pool-domain>/oauth2/idpresponse
      ```

   1. Choose **Add**, and then **Save**\. You do not need to verify the domain\.

   1. Choose **Continue**, and then choose **Register**\.

1. On the left navigation bar, choose **Keys**\.

1. On the **Keys** page, choose the **\+** icon\.

1. On the **Register a New Key** page, do the following:

   1. Under **Key Name**, enter a key name\. 

   1. Choose **Sign In with Apple**, and then choose **Configure**\.

   1. On the **Configure Key** page, choose a **Primary App ID**, and then choose **Save**\.

   1. Choose **Continue**, and then choose **Register**\.

1. On the **Download Your Key** page, choose **Download** to download the private key and note the **Key ID** shown, and then choose **Done**\. You will need this private key and the **Key ID** value shown on this page after you choose Apple as your identity provider in [Step 2: Add a social IdP to your user pool](cognito-user-pools-social-idp.md#cognito-user-pools-social-idp-step-2)\.

## Step 2: Add a social IdP to your user pool<a name="cognito-user-pools-social-step-2"></a>

In this section, you configure a social IdP in your user pool using the client ID and client secret from the previous section\.

------
#### [ Original console ]

**To configure a user pool social identity provider with the AWS Management Console**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose a social identity provider: **Facebook**, **Google**, **Login with Amazon**, or **Apple**\.

1. Choose from the following steps, based on your choice of social identity provider:
   + **Google** and **Login with Amazon** — Enter the **app client ID** and **app client secret** generated in the previous section\.
   + **Facebook** — Enter the **app client ID** and **app client secret** generated in the previous section, and then choose an API version \(for example, version 2\.12\)\. We recommend chosing the latest possible version, as each Facebook API has a lifecycle and deprecation date\. Facebook scopes and attributes can vary between API versions\. We recommend testing your social identity log in with Facebook to ensure that federation works as intended\.
   + **Sign In with Apple** — Enter the **Services ID**, **Team ID**, **Key ID**, and **private key** generated in the previous section\.

1. Enter the names of the scopes that you want to authorize\. Scopes define which user attributes \(such as `name` and `email`\) you want to access with your app\. For Facebook, these should be separated by commas\. For Google and Login with Amazon, they should be separated by spaces\. For Sign in with Apple, select the check boxes for the scopes you want access to\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-social-idp.html)

   Your app user is asked to consent to providing these attributes to your app\. For more information about their scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign in with Apple\. 

   With Sign in with Apple, the following are user scenarios where scopes might not be returned:
   + An end user encounters failures after leaving Apple’s sign in page \(can be from Internal failures within Amazon Cognito or anything written by the developer\)
   + The service ID identifier is used across user pools and/or other authentication services
   + A developer adds additional scopes after the end user has signed in before \(no new information is retrieved\)
   + A developer deletes the user and then the user signs in again without removing the app from their Apple ID profile

1. Choose **Enable** for the social identity provider that you are configuring\.

1. Choose **App client settings** from the navigation bar\.

1. Select your social identity provider as one of the **Enabled Identity Providers** for your user pool app\.

1. Enter your callback URL into **Callback URL\(s\)** for your user pool app\. This is the URL of the page where your user will be redirected after a successful authentication\.

   ```
   https://www.example.com
   ```

1. Choose **Save changes**\.

1. On the **Attribute mapping** tab, add mappings for at least the required attributes, typically `email`, as follows:

   1. Select the check box to choose the Facebook, Google, or Amazon attribute name\. You can also enter the names of additional attributes that are not listed in the Amazon Cognito console\.

   1. Choose the destination user pool attribute from the drop\-down list\.

   1. Choose **Save changes**\.

   1. Choose **Go to summary**\.

------
#### [ New console ]

**To configure a user pool social identity provider with the AWS Management Console**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and select **Add an identity provider**\.

1. Choose a social identity provider: **Facebook**, **Google**, **Login with Amazon**, or **Sign in with Apple**\.

1. 

   Choose from the following steps, based on your choice of social identity provider:
   + **Google** and **Login with Amazon** — Enter the **app client ID** and **app client secret** generated in the previous section\.
   + **Facebook** — Enter the **app client ID** and **app client secret** generated in the previous section, and then choose an API version \(for example, version 2\.12\)\. We recommend chosing the latest possible version, as each Facebook API has a lifecycle and deprecation date\. Facebook scopes and attributes can vary between API versions\. We recommend testing your social identity log in with Facebook to ensure that federation works as intended\.
   + **Sign In with Apple** — Enter the **Services ID**, **Team ID**, **Key ID**, and **private key** generated in the previous section\.

1. Enter the names of the **Authorized scopes** you want to use\. Scopes define which user attributes \(such as `name` and `email`\) you want to access with your app\. For Facebook, these should be separated by commas\. For Google and Login with Amazon, they should be separated by spaces\. For Sign in with Apple, select the check boxes for the scopes you want access to\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-federation-with-social-idp.html)

   Your app user is prompted to consent to providing these attributes to your app\. For more information about social provider scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign in with Apple\. 

   With Sign in with Apple, the following are user scenarios where scopes might not be returned:
   + An end user encounters failures after leaving Apple’s sign in page \(can be from Internal failures within Amazon Cognito or anything written by the developer\)
   + The service ID identifier is used across user pools and/or other authentication services
   + A developer adds additional scopes after the end user has signed in before \(no new information is retrieved\)
   + A developer deletes the user and then the user signs in again without removing the app from their Apple ID profile

1. Map attributes from your identity provider to your user pool\. For more information, see [Specifying Identity Provider Attribute Mappings for Your User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-specifying-attribute-mapping.html)\.

1. Choose **Create**\.

1. From the **App client integration** tab, choose one of the **App clients** in the list and **Edit hosted UI settings**\. Add the new social identity provider to the app client under **Identity providers**\.

1. Choose **Save changes**\.

------

## Step 3: Test your social IdP configuration<a name="cognito-user-pools-social-step-3"></a>

You can create a login URL by using the elements from the previous two sections\. Use it to test your social IdP configuration\.

```
https://<your_user_pool_domain>/login?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com
```

You can find your domain on the user pool **Domain name** console page\. The client\_id is on the **App client settings** page\. Use your callback URL for the **redirect\_uri** parameter\. This is the URL of the page where your user will be redirected after a successful authentication\.

**Note**  
Requests that are not completed within 5 minutes will be cancelled, redirected to the login page, and then display a `Something went wrong` error message\.

## Next step<a name="cognito-user-pools-configuring-federation-with-social-idp-next-step"></a>

[Step 4\. Add sign\-in with a SAML identity provider to a user pool \(optional\)](cognito-user-pools-configuring-federation-with-saml-2-0-idp.md)