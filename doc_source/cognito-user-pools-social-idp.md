# Adding Social Identity Providers to a User Pool<a name="cognito-user-pools-social-idp"></a>

Your web and mobile app users can sign in through social identity providers \(IdP\) like Facebook, Google, Amazon, and Apple\. With the built\-in hosted web UI, Amazon Cognito provides token handling and management for all authenticated users, so your backend systems can standardize on one set of user pool tokens\.

You can add a social identity provider in the AWS Management Console, with the AWS CLI, or using Amazon Cognito API calls\. 

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

**Topics**
+ [Prerequisites](#cognito-user-pools-social-idp-prerequisites)
+ [Step 1: Register with a Social IdP](#cognito-user-pools-social-idp-step-1)
+ [Step 2: Add a Social IdP to Your User Pool](#cognito-user-pools-social-idp-step-2)
+ [Step 3: Test Your Social IdP Configuration](#cognito-user-pools-social-idp-step-3)

## Prerequisites<a name="cognito-user-pools-social-idp-prerequisites"></a>

Before you begin, you need:
+ A user pool with an application client and a user pool domain\. For more information, see [Create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.
+ A social identity provider\.

## Step 1: Register with a Social IdP<a name="cognito-user-pools-social-idp-step-1"></a>

Before you create a social IdP with Amazon Cognito, you must register your application with the social IdP to receive a client ID and client secret\.

### To register an app with Facebook<a name="cognito-user-pools-social-idp-step-1-facebook"></a>

1. Create a [developer account with Facebook](https://developers.facebook.com/docs/facebook-login)\.

1. [Sign in](https://developers.facebook.com/) with your Facebook credentials\.

1.  From the **My Apps** menu, choose **Create New App**\.

1. Give your Facebook app a name and choose **Create App ID**\.

1. On the left navigation bar, choose **Settings** and then **Basic**\.

1. Note the **App ID** and the **App Secret**\. You will use them in the next section\.

1.  Choose **\+ Add Platform** from the bottom of the page\.

1. Choose **Website**\.

1. Under **Website**, type your user pool domain with the /oauth2/idpresponse endpoint into **Site URL**\.

   ```
   https://<your_user_pool_domain>/login?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com
   ```

1. Choose **Save changes**\.

1. Type your user pool domain into **App Domains**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Choose **Save changes**\.

1. From the navigation bar choose **Products** and then **Set up ** from **Facebook Login**\.

1. From the navigation bar choose **Facebook Login** and then **Settings**\.

   Type your redirect URL into **Valid OAuth Redirect URIs**\. It will consist of your user pool domain with the /oauth2/idpresponse endpoint\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Save changes**\.

### To register an app with Amazon<a name="cognito-user-pools-social-idp-step-1-amazon"></a>

1. Create a [developer account with Amazon](https://developer.amazon.com/login-with-amazon)\.

1. [Sign in](https://developer.amazon.com/lwa/sp/overview.html) with your Amazon credentials\.

1. You need to create an Amazon security profile to receive the Amazon client ID and client secret\.

   Choose **Apps and Services** from navigation bar at the top of the page and then choose **Login with Amazon**\.

1. Choose **Create a Security Profile**\.

1. Type in a **Security Profile Name**, a **Security Profile Description**, and a **Consent Privacy Notice URL**\.

1. Choose **Save**\.

1. Choose **Client ID** and **Client Secret** to show the client ID and secret\. You will use them in the next section\.

1. Hover over the gear and choose **Web Settings**, and then choose **Edit**\.

1. Type your user pool domain into **Allowed Origins**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Type your user pool domain with the **/oauth2/idpresponse** endpoint into **Allowed Return URLs**\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Save**\.

### To register an app with Google<a name="cognito-user-pools-social-idp-step-1-google"></a>

1. Create a [developer account with Google](https://developers.google.com/identity)\.

1. [Sign in](https://developers.google.com/identity/sign-in/web/sign-in) with your Google credentials\.

1. Choose **CONFIGURE A PROJECT**\.

1. Type in a project name and choose **NEXT**\.

1. Type in your product name and choose **NEXT**\.

1. Choose **Web browser** from the ** Where are you calling from?** drop\-down list\.

1. Type your user pool domain into **Authorized JavaScript origins**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Choose **CREATE**\. You will not use the **Client ID** and **Client Secret** from this step\.

1. Choose **DONE**\.

1. [Sign in](https://console.developers.google.com) to the Google Console\.

1. On the left navigation bar, choose **Credentials**\.

1. Create your OAuth 2\.0 credentials by choosing **OAuth client ID** from the **Create credentials** drop\-down list\.

1. Choose **Web application**\.

1. Type your user pool domain into **Authorized JavaScript origins**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Type your user pool domain with the **/oauth2/idpresponse** endpoint into **Authorized Redirect URIs**\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Create** twice\.

1. Note the **OAuth client ID** and **client secret**\. You will need them for the next section\.

1. Choose **OK**\.

### To register an app with Apple<a name="cognito-user-pools-social-idp-step-1-apple"></a>

1. Create a [developer account with Apple](https://developer.apple.com/programs/enroll/)\.

1. [Sign in](https://developer.apple.com/account/#/welcome) with your Apple credentials\.

1. On the left navigation bar, choose **Certificates, IDs & Profiles**\.

1. On the left navigation bar, choose **Identifiers**\.

1. On the **Identifiers** page, choose the **\+** icon\.

1. On the **Register a New Identifier** page, choose **App IDs**, and then choose **Continue**\.

1. On the **Register an App ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **App ID Prefix**, type an identifier\. Make a note of the value under **App ID Prefix** as you will need this value after you choose Apple as your identity provider in [Step 2: Add a Social IdP to Your User Pool](#cognito-user-pools-social-idp-step-2)\.

   1. Under **Capabilities**, choose **Sign In with Apple**, and then choose **Edit**\.

   1. On the **Sign in with Apple: App ID Configuration** page, select the appropriate setting for you app, and then choose **Save**\.

   1. Choose **Continue**\.

1. On the **Confirm your App ID** page, choose **Register**\.

1. On the **Identifiers** page, hover over **App IDs** on the right side of the page, choose **Services IDs**, and then choose the **\+** icon\.

1. On the **Register a New Identifier** page, choose **Services IDs**, and then choose **Continue**\.

1. On the **Register a Services ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **Identifier**, type an identifier\. Make a note of this Services ID as you will need this value after you choose Apple as your identity provider in [Step 2: Add a Social IdP to Your User Pool](#cognito-user-pools-social-idp-step-2)\.

   1. Select **Sign In with Apple**, and then choose **Configure**\.

   1. On the **Web Authentication Configuration** page, choose a **Primary App ID**\. Under **Web Domain**, type your user pool domain\. Under **Return URLs**, type your user pool domain and include the /oauth2/idpresponse endpoint\. For example:

      ```
      https://<your-user-pool-domain>/oauth2/idpresponse
      ```

   1. Choose **Add**, and then **Save**\. You do not need to verify the domain\.

   1. Choose **Continue**, and then choose **Register**\.

1. On the left navigation bar, choose **Keys**\.

1. On the **Keys** page, choose the **\+** icon\.

1. On the **Register a New Key** page, do the following:

   1. Under **Key Name**, type a key name\. 

   1. Choose **Sign In with Apple**, and then choose **Configure**\.

   1. On the **Configure Key** page, choose a **Primary App ID**, and then choose **Save**\.

   1. Choose **Continue**, and then choose **Register**\.

1. On the **Download Your Key** page, choose **Download** to download the private key, and then choose **Done**\. You will need this private key and the **Key ID** value shown on this page after you choose Apple as your identity provider in [Step 2: Add a Social IdP to Your User Pool](#cognito-user-pools-social-idp-step-2)\.

## Step 2: Add a Social IdP to Your User Pool<a name="cognito-user-pools-social-idp-step-2"></a>

In this section, you configure a social IdP in your user pool using the client ID and client secret from the previous section\.

**To configure a user pool social identity provider with the AWS Management Console**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **Manage your User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose a social identity provider: **Facebook**, **Google**, **Login with Amazon**, or **Apple**\.

1. For Google and Login with Amazon, type the app client ID and app client secret that you received from the social identity provider in the previous section\. For Facebook, type the app client ID, app client secret that you received from the social identity provider in the previous section, and choose an API version\. We recommend choosing the highest available possible version as each Facebook API version has a lifecycle and a deprecation date for example, version 2\.12\. You can change the API version post creation if you encounter any issues\. The Facebook scopes and attributes may vary with each API version, so we recommend testing your integration\.” For Sign in with Apple, provide the Services ID, Team ID, Key ID, and private key that you received in the previous section\.

1. Type the names of the scopes that you want to authorize\. Scopes define which user attributes \(such as `name` and `email`\) you want to access with your app\. For Facebook, these should be separated by commas\. For Google and Login with Amazon, they should be separated by spaces\. For Sign in with Apple, select the check boxes for the scopes you want access to\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-social-idp.html)

   Your app user is asked to consent to providing these attributes to your app\. For more information about their scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign in with Apple\. 

   In the case of Sign in with Apple, the following are user scenarios where scopes might not be returned:
   + An end user encounters failures after leaving Apple’s sign in page \(can be from Internal failures within Cognito or anything written by the developer\)
   + The service id identifier is used across user pools and/or other authentication services
   + A developer adds additional scopes after the end user has signed in before \(no new information is retrieved\)
   + A developer deletes the user and then the user signs in again without removing the app from their Apple ID profile

1. Choose **Enable** for the social identity provider that you are configuring\.

1. Choose **App client settings** from the navigation bar\.

1. Select your social identity provider as one of the **Enabled Identity Providers** for your user pool app\.

1. Type your callback URL into **Callback URL\(s\)** for your user pool app\. This is the URL of the page where your user will be redirected after a successful authentication\.

   ```
   https://www.example.com
   ```

1. Choose **Save changes**\.

1. On the **Attribute mapping** tab, add mappings for at least the required attributes, typically `email`, as follows:

   1. Select the check box to choose the Facebook, Google, or Amazon attribute name\. You can also type the names of additional attributes that are not listed in the Amazon Cognito console\.

   1. Choose the destination user pool attribute from the drop\-down list\.

   1. Choose **Save changes**\.

   1. Choose **Go to summary**\.

## Step 3: Test Your Social IdP Configuration<a name="cognito-user-pools-social-idp-step-3"></a>

You can create a login URL by using the elements from the previous two sections\. Use it to test your social IdP configuration\.

```
https://<your_user_pool_domain>/login?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com
```

You can find your domain on the user pool **Domain name** console page\. The client\_id is on the **App client settings** page\. Use your callback URL for the **redirect\_uri** parameter\. This is the URL of the page where your user will be redirected after a successful authentication\.

**Note**  
Requests that are not completed within 5 minutes will be cancelled, redirected to the login page, and then display a `Something went wrong` error message\.