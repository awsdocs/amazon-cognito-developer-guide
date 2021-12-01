# Facebook \(identity pools\)<a name="facebook"></a>

 Amazon Cognito identity pools integrate with Facebook to provide federated authentication for your mobile application users\. This section explains how to register and set up your application with Facebook as an identity provider\. 

## Set up Facebook<a name="set-up-facebook"></a>

 You need to register your application with Facebook before you can start authenticating Facebook users and interacting with Facebook APIs\. 

 The [Facebook Developers portal](https://developers.facebook.com/) takes you through the process of setting up your application\. If you haven't gone through that process yet, you must to do so before you can integrate Facebook in your Amazon Cognito identity pool: 

**To set up Facebook**

1.  At the [Facebook Developers portal](https://developers.facebook.com/), log in with your Facebook credentials\. 

1.  From the **Apps** menu, select **Add a New App**\. 

1.  Select a platform and complete the quick start process\. 

### Android<a name="set-up-facebook-1.android"></a>

 The [Facebook Getting Started Guide](https://developers.facebook.com/docs/android/getting-started) provides additional information on integrating with Facebook Login\. 

### iOS \- Objective\-C<a name="set-up-facebook-1.ios-objc"></a>

 The [Facebook Getting Started Guide](https://developers.facebook.com/docs/ios/getting-started/) provides additional information about integrating with Facebook Login\. 

### iOS \- Swift<a name="set-up-facebook-1.ios-swift"></a>

 The [Facebook Getting Started Guide](https://developers.facebook.com/docs/ios/getting-started/) provides additional information about integrating with Facebook Login\. 

### JavaScript<a name="set-up-facebook-1.javascript"></a>

 The [Facebook Getting Started Guide](https://developers.facebook.com/docs/facebook-login/login-flow-for-web/v2.3) provides additional information about integrating with Facebook Login\. 

### Unity<a name="set-up-facebook-1.unity"></a>

 The [Facebook Getting Started Guide](https://developers.facebook.com/docs/unity) provides additional information about integrating with Facebook Login\. 

### Xamarin<a name="set-up-facebook-1.xamarin"></a>

 To provide Facebook authentication, first follow the appropriate flow below to include and set up the Facebook SDK in your application\. Amazon Cognito identity pools use the Facebook access token to generate a unique user identifier that is associated with an Amazon Cognito identity\. 
+  [Facebook iOS SDK by Xamarin](http://components.xamarin.com/view/facebookios) 
+  [Facebook Android SDK by Xamarin](http://components.xamarin.com/view/facebookandroid) 

## Configure the external provider in the Amazon Cognito federated identities console<a name="configure-the-external-provider-in-the-amazon-cognito-console"></a>

Use the following procedure to configure your external provider\.

1. Choose **Manage Identity Pools** from the [Amazon Cognito console home page](https://console.aws.amazon.com/cognito/home)\.

1. Choose the name of the identity pool for which you want to enable Facebook as an external provider\. The **Dashboard** page for your identity pool appears\. 

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The **Edit identity pool** page appears\. 

1. Scroll down and choose **Authentication providers** to expand it\. 

1. Choose the **Facebook** tab\. 

1. Choose **Unlock**\. 

1. Enter the Facebook App ID you obtained from Facebook, and then choose **Save Changes**\. 

## Using Facebook<a name="using-facebook"></a>

### Android<a name="using-facebook-1.android"></a>

 To provide Facebook authentication, first follow the [Facebook guide](https://developers.facebook.com/docs/android) to include their SDK in your application\. Then add a ["Login with Facebook" button](https://developers.facebook.com/docs/facebook-login/android) to your Android user interface\. The Facebook SDK uses a session object to track its state\. Amazon Cognito uses the access token from this session object to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\. 

 Once you have authenticated your user with the Facebook SDK, add the session token to the Amazon Cognito credentials provider\. 

 Facebook SDK 4\.0 or later: 

```
Map<String, String> logins = new HashMap<String, String>();
logins.put("graph.facebook.com", AccessToken.getCurrentAccessToken().getToken());
credentialsProvider.setLogins(logins);
```

 Facebook SDK before 4\.0: 

```
Map<String, String> logins = new HashMap<String, String>();
logins.put("graph.facebook.com", Session.getActiveSession().getAccessToken());
credentialsProvider.setLogins(logins);
```

 The Facebook login process initializes a singleton session in its SDK\. The Facebook session object contains an OAuth token that Amazon Cognito uses to generate AWS credentials for your authenticated end user\. Amazon Cognito also uses the token to check against your user database for the existence of a user matching this particular Facebook identity\. If the user already exists, the API returns the existing identifier\. Otherwise a new identifier is returned\. Identifiers are automatically cached by the client SDK on the local device\. 

**Note**  
After setting the logins map, you must make a call to `refresh` or `get` to actually get the AWS credentials\.

### iOS \- Objective\-C<a name="using-facebook-1.ios-objc"></a>

 To add Facebook authentication, first follow the [Facebook guide](https://developers.facebook.com/docs/ios) to integrate the Facebook SDK into your application\. Then add a [Login with Facebook button](https://developers.facebook.com/docs/facebook-login/ios) to your user interface\. The Facebook SDK uses a session object to track its state\. Amazon Cognito uses the access token from this session object to authenticate the user and bind them to a unique Amazon Cognito identity pools \(federated identities\)\. 

 To provide the Facebook access token to Amazon Cognito, implement the [AWSIdentityProviderManager](https://docs.aws.amazon.com/AWSiOSSDK/latest/Protocols/AWSIdentityProviderManager.html) protocol\. 

 In the implementation of the `logins` method, return a dictionary containing `AWSIdentityProviderFacebook`\. This dictionary acts as the key and the current access token from the authenticated Facebook user as the value, as shown in the following code example\. 

```
- (AWSTask<NSDictionary<NSString *, NSString *> *> *)logins {
    FBSDKAccessToken* fbToken = [FBSDKAccessToken currentAccessToken];
    if(fbToken){
        NSString *token = fbToken.tokenString;
        return [AWSTask taskWithResult: @{ AWSIdentityProviderFacebook : token }];
    }else{
            return [AWSTask taskWithError:[NSError errorWithDomain:@"Facebook Login"
                                                          code:-1
                                                      userInfo:@{@"error":@"No current Facebook access token"}]];
    }
}
```

 When you instantiate the `AWSCognitoCredentialsProvider`, pass the class that implements `AWSIdentityProviderManager` as the value of `identityProviderManager` in the constructor\. For more information, go to the [AWSCognitoCredentialsProvider](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProvider.html) reference page and choose **initWithRegionType:identityPoolId:identityProviderManager**\.

### iOS \- Swift<a name="using-facebook-1.ios-swift"></a>

 To add Facebook authentication, first follow the [Facebook guide](https://developers.facebook.com/docs/ios) to integrate the Facebook SDK into your application\. Then add a [Login with Facebook button](https://developers.facebook.com/docs/facebook-login/ios) to your user interface\. The Facebook SDK uses a session object to track its state\. Amazon Cognito uses the access token from this session object to authenticate the user and bind them to a unique Amazon Cognito identity pools \(federated identities\)\. 

 To provide the Facebook access token to Amazon Cognito, implement the [AWSIdentityProviderManager](https://docs.aws.amazon.com/AWSiOSSDK/latest/Protocols/AWSIdentityProviderManager.html) protocol\. 

 In the implementation of the `logins` method, return a dictionary containing `AWSIdentityProviderFacebook`\. This dictionary acts as the key and the current access token from the authenticated Facebook user as the value, as shown in the following code example\. 

```
class FacebookProvider: NSObject, AWSIdentityProviderManager {
    func logins() -> AWSTask<NSDictionary> {
        if let token = AccessToken.current?.authenticationToken {
            return AWSTask(result: [AWSIdentityProviderFacebook:token])
        }
        return AWSTask(error:NSError(domain: "Facebook Login", code: -1 , userInfo: ["Facebook" : "No current Facebook access token"]))
    }
}
```

 When you instantiate the `AWSCognitoCredentialsProvider`, pass the class that implements `AWSIdentityProviderManager` as the value of `identityProviderManager` in the constructor\. For more information, go to the [AWSCognitoCredentialsProvider](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProvider.html) reference page and choose **initWithRegionType:identityPoolId:identityProviderManager**\.

### JavaScript<a name="using-facebook-1.javascript"></a>

To provide Facebook authentication, follow the [Facebook Login for the Web](https://developers.facebook.com/docs/facebook-login/login-flow-for-web/v2.3) to add the "Login with Facebook" button on your website\. The Facebook SDK uses a session object to track its state\. Amazon Cognito uses the access token from this session object to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\. 

 Once you have authenticated your user with the Facebook SDK, add the session token to the Amazon Cognito credentials provider\. 

```
FB.login(function (response) {

  // Check if the user logged in successfully.
  if (response.authResponse) {

    console.log('You are now logged in.');

    // Add the Facebook access token to the Amazon Cognito credentials login map.
    AWS.config.credentials = new AWS.CognitoIdentityCredentials({
      IdentityPoolId: 'IDENTITY_POOL_ID',
      Logins: {
        'graph.facebook.com': response.authResponse.accessToken
      }
    });

    // Obtain AWS credentials
    AWS.config.credentials.get(function(){
        // Access AWS resources here.
    });

  } else {
    console.log('There was a problem logging you in.');
  }

});
```

 The Facebook SDK obtains an OAuth token that Amazon Cognito uses to generate AWS credentials for your authenticated end user\. Amazon Cognito also uses the token to check against your user database for the existence of a user matching this particular Facebook identity\. If the user already exists, the API returns the existing identifier\. Otherwise a new identifier is returned\. Identifiers are automatically cached by the client SDK on the local device\. 

**Note**  
After setting the logins map, you must make a call to `refresh` or `get` to get the credentials\. For a code example, see "Use Case 17, Integrating User Pools with Cognito Identity," in the [JavaScript README file](https://github.com/aws/aws-amplify/tree/master/packages/amazon-cognito-identity-js/README.md)\.

### Unity<a name="using-facebook-1.unity"></a>

 To provide Facebook authentication, first follow the [Facebook guide](https://developers.facebook.com/docs/unity) to include and set up their SDK in your application\. Amazon Cognito uses the Facebook access token from the 'FB' object to generate a unique user identifier that is associated with an Amazon Cognito identity\. 

 Once you have authenticated your user with the Facebook SDK, add the session token to the Amazon Cognito credentials provider: 

```
void Start()
{
    FB.Init(delegate() {
        if (FB.IsLoggedIn) { //User already logged in from a previous session
            AddFacebookTokenToCognito();
        } else {
            FB.Login ("email", FacebookLoginCallback);
        }
    });
}

void FacebookLoginCallback(FBResult result)
{
    if (FB.IsLoggedIn)
    {
        AddFacebookTokenToCognito();
    }
    else
    {
        Debug.Log("FB Login error");
    }
}

void AddFacebookTokenToCognito()
{
    credentials.AddLogin ("graph.facebook.com", AccessToken.CurrentAccessToken.TokenString);
}
```

 You should make sure to call `FB.Login()` and that `FB.IsLoggedIn` is true before using `FB.AccessToken`\. 

### Xamarin<a name="using-facebook-1.xamarin"></a>

 **Xamarin for Android:** 

```
public void InitializeFacebook() {
    FacebookSdk.SdkInitialize(this.ApplicationContext);
    callbackManager = CallbackManagerFactory.Create();
    LoginManager.Instance.RegisterCallback(callbackManager, new FacebookCallback &lt; LoginResult &gt; () {
      HandleSuccess = loginResult = &gt; {
        var accessToken = loginResult.AccessToken;
        credentials.AddLogin("graph.facebook.com", accessToken.Token);
        //open new activity
      },
      HandleCancel = () = &gt; {
        //throw error message
      },
      HandleError = loginError = &gt; {
        //throw error message
      }
    });
    LoginManager.Instance.LogInWithReadPermissions(this, new List &lt; string &gt; {
      "public_profile"
    });
  }
```

 **Xamarin for iOS:** 

```
public void InitializeFacebook() {
  LoginManager login = new LoginManager();
  login.LogInWithReadPermissions(readPermissions.ToArray(), delegate(LoginManagerLoginResult result, NSError error) {
    if (error != null) {
      //throw error message
    } else if (result.IsCancelled) {
      //throw error message
    } else {
      var accessToken = loginResult.AccessToken;
      credentials.AddLogin("graph.facebook.com", accessToken.Token);
      //open new view controller
    }
  });
}
```