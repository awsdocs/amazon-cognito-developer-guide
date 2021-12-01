# Google \(identity pools\)<a name="google"></a>

Amazon Cognito integrates with Google to provide federated authentication for your mobile application users\. This section explains how to register and set up your application with Google as an identity provider\.

## Android<a name="set-up-google-1.android"></a>

**Note**  
If your app uses Google and will be available on multiple mobile platforms, you should configure it as an [OpenID Connect Provider](open-id.md)\. Adding all created client IDs as additional audience values to allow for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

**Set Up Google**

To enable Google Sign\-in for Android, you must create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Under **APIs and auth > APIs > Social APIs**, enable the Google API\.

1. Under **APIs and auth > Credentials > OAuth consent screen**, create the dialog that will be shown to users when your app requests access to their private data\.

1. Under **Credentials > Add Credentials**, create an OAuth 2\.0 client ID for Android\. You will need a client ID for each platform you intend to develop for \(e\.g\. web, iOS, Android\)\.

1. Under **Credentials > Add Credentials**, create a Service Account\. The console will alert you that a new public/private key has been created\.

For additional instructions on using the Google Developers console, see [Managing projects in the Developers Console](https://developers.google.com/console/help/new/?hl=en)\.

For more information on integrating Google into your Android app, see the [Google documentation for Android](https://developers.google.com/identity/sign-in/android/start-integrating)\.

**Configure the External Provider in the Amazon Cognito Console**

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool for which you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the [Google documentation for Android](https://developers.google.com/identity/sign-in/android/start)\. Successful authentication results in an OpenID Connect authentication token, which Amazon Cognito uses to authenticate the user and generate a unique identifier\.

The following sample code shows how to retrieve the authentication token from the Google Play service:

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
If your app uses Google and will be available on multiple mobile platforms, you should configure it as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values to allow for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

To enable Google Sign\-in for iOS, you must create a Google Developers console project for your application\.

**Set Up Google**

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Under **APIs and auth > APIs > Social APIs**, enable the Google API\.

1. Under **APIs and auth > Credentials > OAuth consent screen**, create the dialog that will be shown to users when your app requests access to their private data\.

1. Under **Credentials > Add Credentials**, create an OAuth 2\.0 client ID for iOS\. You will need a client ID for each platform you intend to develop for \(e\.g\. web, iOS, Android\)\.

1. Under **Credentials > Add Credentials**, create a Service Account\. The console will alert you that a new public/private key has been created\.

For additional instructions on using the Google Developers console, see [Managing projects in the Developers Console](https://developers.google.com/console/help/new/?hl=en)\.

For more information on integrating Google into your iOS app, see the [Google documentation for iOS](https://developers.google.com/identity/sign-in/ios/start-integrating)\.

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

**Configure the External Provider in the Amazon Cognito Console**

1. Choose the name of the identity pool for which you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

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
If your app uses Google and will be available on multiple mobile platforms, you should configure it as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values to allow for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

To enable Google Sign\-in for iOS, you will need to create a Google Developers console project for your application\.

**Set Up Google**

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Under **APIs and auth > APIs > Social APIs**, enable the Google API\.

1. Under **APIs and auth > Credentials > OAuth consent screen**, create the dialog that will be shown to users when your app requests access to their private data\.

1. Under **Credentials > Add Credentials**, create an OAuth 2\.0 client ID for iOS\. You will need a client ID for each platform you intend to develop for \(e\.g\. web, iOS, Android\)\.

1. Under **Credentials > Add Credentials**, create a Service Account\. The console will alert you that a new public/private key has been created\.

For additional instructions on using the Google Developers console, see [Managing projects in the Developers Console](https://developers.google.com/console/help/new/?hl=en)\.

For more information on integrating Google into your iOS app, see the [Google documentation for iOS](https://developers.google.com/identity/sign-in/ios/start-integrating)\.

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

**Configure the External Provider in the Amazon Cognito Console**

1. Choose the name of the identity pool for which you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the [Google documentation for iOS](https://developers.google.com/identity/sign-in/ios/start/)\. Successful authentication results in an OpenID Connect authentication token, which Amazon Cognito uses to authenticate the user and generate a unique identifier\.

Successful authentication results in a `GTMOAuth2Authentication` object, which contains an `id_token`, which Amazon Cognito uses to authenticate the user and generate a unique identifier:

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
If your app uses Google and will be available on multiple mobile platforms, you should configure it as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values to allow for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.

**Set Up Google**

To enable Google Sign\-in for your web application, you will need to create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Under **APIs and auth > APIs > Social APIs**, enable the Google API\.

1. Under **APIs and auth > Credentials > OAuth consent screen**, create the dialog that will be shown to users when your app requests access to their private data\.

1. Under **Credentials > Add Credentials**, create an OAuth 2\.0 client ID for your web application\. You will need a client ID for each platform you intend to develop for \(such as web, iOS, Android\)\.

1. Under **Credentials > Add Credentials**, create a Service Account\. The console alert you that a new public/private key has been created\.

For additional instructions on using the Google Developers console, see [Managing projects in the Developers Console](https://developers.google.com/console/help/new/?hl=en)\.

**Configure the External Provider in the Amazon Cognito Console**

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool for which you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

**Use Google**

To enable login with Google in your application, follow the [Google documentation for Web](https://developers.google.com/identity/sign-in/web/sign-in)\.

Successful authentication results in a response object that contains an `id_token`, which Amazon Cognito uses to authenticate the user and generate a unique identifier:

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

**Set Up Google**

To enable Google Sign\-in for your web application, you will need to create a Google Developers console project for your application\.

1. Go to the [Google Developers console](https://console.developers.google.com/) and create a new project\.

1. Under **APIs and auth > APIs > Social APIs**, enable the Google API\.

1. Under **APIs and auth > Credentials > OAuth consent screen**, create the dialog that will be shown to users when your app requests access to their private data\.

1. For Unity, you need to create a total of three IDs: two for Android and one for iOS\. Under **Credentials > Add Credentials**: 
   + Android: Create an OAuth 2\.0 client ID for Android and an OAuth 2\.0 client ID for a web application\.
   + iOS: Create an OAuth 2\.0 client ID for iOS\.

1. Under **Credentials > Add Credentials**, create a Service Account\. The console will alert you that a new public/private key has been created\.

**Create an OpenID Provider in the IAM Console**

1. Next, you will need to create an OpenID Provider in the IAM Console\. For instructions on how to set up an OpenID Provider, see [Using OpenID Connect Identity Providers](open-id.md)\.

1. When prompted for your Provider URL, enter `"https://accounts.google.com"`\.

1. When prompted to enter a value in the **Audience** field, enter any one of the three client IDs that you created in the previous steps\.

1. After creating the provider, choose the provider name and add two more audiences, providing the two remaining client IDs\.

**Configure the External Provider in the Amazon Cognito Console**

Choose **Manage Identity Pools** from the [Amazon Cognito Console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool for which you want to enable Google as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Google** tab\.

1. Choose **Unlock**\.

1. Enter the Google Client ID you obtained from Google, and then choose **Save Changes**\.

**Install the Unity Google Plugin**

1. Add the [Google Play Games plugin for Unity](https://github.com/playgameservices/play-games-plugin-for-unity) to your Unity project\.

1. In Unity, from the **Windows** menu, configure the plugin using the three IDs for the Android and iOS platforms\.

**Use Google**

The following sample code shows how to retrieve the authentication token from the Google Play service:

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
Google integration is not natively supported on the Xamarin platform\. Integration currently requires the use of a web view to go through the browser sign\-in flow\. To learn how Google integration works with other SDKs, please select another platform\.

To enable login with Google in your application, you will need to authenticate your users and obtain an OpenID Connect token from them\. Amazon Cognito uses this token to generate a unique user identifier that is associated with an Amazon Cognito identity\. Unfortunately, the Google SDK for Xamarin doesn't allow you to retrieve the OpenID Connect token, so you must use an alternative client or the web flow in a web view\.

Once you have the token, you can set it in your `CognitoAWSCredentials`:

```
credentials.AddLogin("accounts.google.com", token);
```

**Note**  
If your app uses Google and will be available on multiple mobile platforms, you should configure it as an [OpenID Connect Provider](open-id.md)\. Add all created client IDs as additional audience values to allow for better integration\. To learn more about Google's cross\-client identity model, see [Cross\-client Identity](https://developers.google.com/accounts/docs/CrossClientAuth)\.