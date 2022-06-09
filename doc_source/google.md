# Google \(identity pools\)<a name="google"></a>

Amazon Cognito integrates with Google to provide federated authentication for your mobile application users\. This section explains how to register and set up your application with Google as an IdP\.

## Android<a name="set-up-google-1.android"></a>

**Note**  
If your app uses Google and is available on multiple mobile platforms, you should configure it as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

**Setting up Google**

To activate Google Sign\-in for Android, create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Choose **APIs & Services**, then **OAuth consent screen**\. Customize the information that Google shows to your users when Google asks for their consent to share their profile data with your app\.

1. Choose **Credentials**, then **Create credentials**\. Choose **OAuth client ID**\. Select **Android** as the **Application type**\. Create a separate client ID for each platform where you develop your app\.

1. From **Credentials**, choose **Manage service accounts**\. Choose **Create service account**\. Enter your service account details, and then choose **Create and continue**\.

1. Grant the service account access to your project\. Grant users access to the service account as your app requires\.

1. Choose your new service account, choose the **Keys** tab, and **Add key**\. Create and download a new JSON key\.

For more information about how to use the Google Developers console, see [Creating and managing projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) in the Google Cloud documentation\.

For more information about how to integrate Google into your Android app, see [Google Sign\-In for Android](https://developers.google.com/identity/sign-in/android/start-integrating) in the Google Identity documentation\.

**Configuring the external provider in the Amazon Cognito Console**

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool where you want to activate Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers**\. The Edit identity pool page expands to show additional Authentication provider options\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID that you created in the Google Cloud console, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the instructions in the [Google documentation for Android](https://developers.google.com/identity/sign-in/android/start)\. When a user signs in, they request an OpenID Connect authentication token from Google\. Amazon Cognito then uses the token to authenticate the user and generate a unique identifier\.

The following example code shows how to retrieve the authentication token from the Google Play service:

```
GooglePlayServicesUtil.isGooglePlayServicesAvailable(getApplicationContext());
AccountManager am = AccountManager.get(this);
Account[] accounts = am.getAccountsByType(GoogleAuthUtil.GOOGLE_ACCOUNT_TYPE);
String token = GoogleAuthUtil.getToken(getApplicationContext(), accounts[0].name,
        "audience:server:client_id:YOUR_GOOGLE_CLIENT_ID");
Map<String, String> logins = new HashMap<String, String>();
logins.put("accounts.google.com", token);
credentialsProvider.setLogins(logins);
```

## iOS \- Objective\-C<a name="set-up-google-1.ios-objc"></a>

**Note**  
If your app uses Google and is available on multiple mobile platforms, configure Google as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

**Setting up Google**

To enable Google Sign\-in for iOS, create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Choose **APIs & Services**, then **OAuth consent screen**\. Customize the information that Google shows to your users when Google asks for their consent to share their profile data with your app\.

1. Choose **Credentials**, then **Create credentials**\. Choose **OAuth client ID**\. Select **iOS** as the **Application type**\. Create a separate client ID for each platform where you develop your app\.

1. From **Credentials**, choose **Manage service accounts**\. Choose **Create service account**\. Enter your service account details, and choose **Create and continue**\.

1. Grant the service account access to your project\. Grant users access to the service account as your app requires\.

1. Choose your new service account\. Choose the **Keys** tab, and **Add key**\. Create and download a new JSON key\.

For more information about how to use the Google Developers console, see [Creating and managing projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) in the Google Cloud documentation\.

For more information about how to integrate Google into your iOS app, see [Google Sign\-In for iOS](https://developers.google.com/identity/sign-in/ios/start-integrating) in the Google Identity documentation\.

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

**Configuring the external provider in the Amazon Cognito Console**

1. Choose the name of the identity pool where you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand the section\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID that you obtained from Google, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the [Google documentation for iOS](https://developers.google.com/identity/sign-in/ios/start/)\. Successful authentication results in an OpenID Connect authentication token, which Amazon Cognito uses to authenticate the user and generate a unique identifier\.

Successful authentication results in a `GTMOAuth2Authentication` object, which contains an `id_token`, which Amazon Cognito uses to authenticate the user and generate a unique identifier:

```
- (void)finishedWithAuth: (GTMOAuth2Authentication *)auth error: (NSError *) error {
        NSString *idToken = [auth.parameters objectForKey:@"id_token"];
        credentialsProvider.logins = @{ @(AWSCognitoLoginProviderKeyGoogle): idToken };
    }
```

## iOS \- Swift<a name="set-up-google-1.ios-swift"></a>

**Note**  
If your app uses Google and is available on multiple mobile platforms, configure Google as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

**Setting up Google**

To enable Google Sign\-in for iOS, create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Choose **APIs & Services**, then **OAuth consent screen**\. Customize the information that Google shows to your users when Google asks for their consent to share their profile data with your app\.

1. Choose **Credentials**, then **Create credentials**\. Choose **OAuth client ID**\. Select **iOS** as the **Application type**\. Create a separate client ID for each platform where you develop your app\.

1. From **Credentials**, choose **Manage service accounts**\. Choose **Create service account**\. Enter your service account details, and choose **Create and continue**\.

1. Grant the service account access to your project\. Grant users access to the service account as your app requires\.

1. Choose your new service account, choose the **Keys** tab, and **Add key**\. Create and download a new JSON key\.

For more information about how to use the Google Developers console, see [Creating and managing projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) in the Google Cloud documentation\.

For more information about how to integrate Google into your iOS app, see [Google Sign\-In for iOS](https://developers.google.com/identity/sign-in/ios/start-integrating) in the Google Identity documentation\.

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

**Configuring the external provider in the Amazon Cognito Console**

1. Choose the name of the identity pool where you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand the section\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID that you obtained from Google, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the [Google documentation for iOS](https://developers.google.com/identity/sign-in/ios/start/)\. Successful authentication results in an OpenID Connect authentication token that Amazon Cognito uses to authenticate the user and generate a unique identifier\.

Successful authentication results in a `GTMOAuth2Authentication` object that contains an `id_token`\. Amazon Cognito uses this token to authenticate the user and generate a unique identifier:

```
func finishedWithAuth(auth: GTMOAuth2Authentication!, error: NSError!) {
    if error != nil {
      print(error.localizedDescription)
    }
    else {
      let idToken = auth.parameters.objectForKey("id_token")
      credentialsProvider.logins = [AWSCognitoLoginProviderKey.Google.rawValue: idToken!]
    }
}
```

## JavaScript<a name="set-up-google-1.javascript"></a>

**Note**  
If your app uses Google and is available on multiple mobile platforms, you should configure Google as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

**Setting up Google**

To enable Google Sign\-in for a JavaScript web app, create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Choose **APIs & Services**, then **OAuth consent screen**\. Customize the information that Google shows to your users when Google asks their consent to share their profile data with your app\.

1. Choose **Credentials**, then **Create credentials**\. Choose **OAuth client ID**\. Select **Web application** as the **Application type**\. Create a separate client ID for each platform where you develop your app\.

1. From **Credentials**, choose **Manage service accounts**\. Choose **Create service account**\. Enter your service account details, and choose **Create and continue**\.

1. Grant the service account access to your project\. Grant users access to the service account as your app requires\.

1. Choose your new service account, choose the **Keys** tab, and **Add key**\. Create and download a new JSON key\.

For more information about how to use the Google Developers console, see [Creating and managing projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) in the Google Cloud documentation\.

For more information about how to integrate Google into your web app, see [Sign in With Google](https://developers.google.com/identity/gsi/web/guides/overview) in the Google Identity documentation\.

**Configure the External Provider in the Amazon Cognito Console**

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool where you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand the section\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the [Google documentation for Web](https://developers.google.com/identity/sign-in/web/sign-in)\.

Successful authentication results in a response object that contains an `id_token` that Amazon Cognito uses to authenticate the user and generate a unique identifier:

```
function signinCallback(authResult) {
  if (authResult['status']['signed_in']) {

     // Add the Google access token to the Amazon Cognito credentials login map.
     AWS.config.credentials = new AWS.CognitoIdentityCredentials({
        IdentityPoolId: 'IDENTITY_POOL_ID',
        Logins: {
           'accounts.google.com': authResult['id_token']
        }
     });

     // Obtain AWS credentials
     AWS.config.credentials.get(function(){
        // Access AWS resources here.
     });
  }
}
```

## Unity<a name="set-up-google-1.unity"></a>

**Setting up Google**

To enable Google Sign\-in for a Unity app, create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Choose **APIs & Services**, then **OAuth consent screen**\. Customize the information that Google shows to your users when Google asks for their consent to share their profile data with your app\.

1. Choose **Credentials**, then **Create credentials**\. Choose **OAuth client ID**\. Select **Web application** as the **Application type**\. Create a separate client ID for each platform where you develop your app\.

1. For Unity, create an additional **OAuth client ID** for **Android**, and another for **iOS**\.

1. From **Credentials**, choose **Manage service accounts**\. Choose **Create service account**\. Enter your service account details, and choose **Create and continue**\.

1. Grant the service account access to your project\. Grant users access to the service account as your app requires\.

1. Choose your new service account, choose the **Keys** tab, and **Add key**\. Create and download a new JSON key\.

For more information about how to use the Google Developers console, see [Creating and managing projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) in the Google Cloud documentation\.

**Create an OpenID Provider in the IAM Console**

1. Create an OpenID Provider in the IAM Console\. For information about how to set up an OpenID Provider, see [Using OpenID Connect Identity Providers](open-id.md)\.

1. When prompted for your Provider URL, enter `"https://accounts.google.com"`\.

1. When prompted to enter a value in the **Audience** field, enter any one of the three client IDs that you created in the previous steps\.

1. Choose the provider name and add two more audiences with the two other client IDs\.

**Configure the External Provider in the Amazon Cognito Console**

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool where you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand the section\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

**Install the Unity Google Plugin**

1. Add the [Google Play Games plugin for Unity](https://github.com/playgameservices/play-games-plugin-for-unity) to your Unity project\.

1. In Unity, from the **Windows** menu, use the three IDs for the Android and iOS platforms to configure the plugin\.

**Use Google**

The following example code shows how to retrieve the authentication token from the Google Play service:

```
void Start()
{
  PlayGamesClientConfiguration config = new PlayGamesClientConfiguration.Builder().Build();
  PlayGamesPlatform.InitializeInstance(config);
  PlayGamesPlatform.DebugLogEnabled = true;
  PlayGamesPlatform.Activate();
  Social.localUser.Authenticate(GoogleLoginCallback);
}

void GoogleLoginCallback(bool success)
{
  if (success)
  {
    string token = PlayGamesPlatform.Instance.GetIdToken();
    credentials.AddLogin("accounts.google.com", token);
  }
  else
  {
    Debug.LogError("Google login failed. If you are not running in an actual Android/iOS device, this is expected.");
  }
}
```

## Xamarin<a name="set-up-google-1.xamarin"></a>

**Note**  
Amazon Cognito doesn't natively support Google on the Xamarin platform\. Integration currently requires the use of a web view to go through the browser sign\-in flow\. To learn how Google integration works with other SDKs, please select another platform\.

To enable login with Google in your application, authenticate your users and obtain an OpenID Connect token from them\. Amazon Cognito uses this token to generate a unique user identifier that is associated with an Amazon Cognito identity\. Unfortunately, the Google SDK for Xamarin doesn't allow you to retrieve the OpenID Connect token, so use an alternative client or the web flow in a web view\.

After you have the token, you can set it in your `CognitoAWSCredentials`:

```
credentials.AddLogin("accounts.google.com", token);
```

**Note**  
If your app uses Google and is available on multiple mobile platforms, you should configure Google as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.