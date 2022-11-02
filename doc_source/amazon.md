# Login with Amazon \(identity pools\)<a name="amazon"></a>

Amazon Cognito integrates with Login with Amazon to provide federated authentication for your mobile and web app users\. This section explains how to register and set up your application with Login with Amazon as an identity provider \(IdP\)\.

Set up Login with Amazon to work with Amazon Cognito in the [Developer Portal](https://developer.amazon.com/login-with-amazon)\. For more information, see [Setting Up Login with Amazon](https://developer.amazon.com/docs/login-with-amazon/faq.html#setting-up-login-with-amazon) in the Login with Amazon FAQ\.

**Note**  
To integrate Login with Amazon into a Xamarin application, follow the [Xamarin Getting Started Guide](https://developer.xamarin.com/guides/cross-platform/getting_started/)\.

**Note**  
You can't natively integrate Login with Amazon on the Unity platform\. Instead, use a web view and go through the browser sign\-in flow\.

## Setting up Login with Amazon<a name="login-with-amazon-setup"></a>

**Implement Login with Amazon **

In the [Amazon developer portal](https://developer.amazon.com/apps-and-games/login-with-amazon), you can set up an OAuth application to integrate with your identity pool, find Login with Amazon documentation, and download SDKs\. Choose **Developer console**, then **Login with Amazon** in the developer portal\. You can create a security profile for your application and then build Login with Amazon authentication mechanisms into your app\. See [Getting credentials](getting-credentials.md) for more information about how to integrate Login with Amazon authentication with your app\.

Amazon issues an OAuth 2\.0 **client ID** for your new security profile\. You can find the **client ID** on the security profile **Web Settings** tab\. Enter the **client ID** in the **Amazon App ID** field of the Login with Amazon IdP in your identity pool\.

## Configure the external provider in the Amazon Cognito console<a name="login-with-amazon-configure-provider"></a>

Choose **Manage Identity Pools** from the [Amazon Cognito console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool where you want to enable Login with Amazon as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand the section\.

1. Choose the **Amazon** tab\.

1. Choose **Unlock**\.

1. Enter the Amazon App ID you obtained from Amazon, and then choose **Save Changes**\.

## Use Login with Amazon: Android<a name="set-up-amazon-1.android"></a>

After you authenticate Amazon login, you can pass the token to the Amazon Cognito credentials provider in the onSuccess method of the TokenListener interface\. The code looks like this:

```
@Override
public void onSuccess(Bundle response) {
    String token = response.getString(AuthzConstants.BUNDLE_KEY.TOKEN.val);
    Map<String, String> logins = new HashMap<String, String>();
    logins.put("www.amazon.com", token);
    credentialsProvider.setLogins(logins);
}
```

## Use Login with Amazon: iOS \- Objective\-C<a name="set-up-amazon-1.ios-objc"></a>

After you authenticate Amazon login, you can pass the token to the Amazon Cognito credentials provider in the requestDidSucceed method of the AMZNAccessTokenDelegate:

```
- (void)requestDidSucceed:(APIResult \*)apiResult {
    if (apiResult.api == kAPIAuthorizeUser) {
        [AIMobileLib getAccessTokenForScopes:[NSArray arrayWithObject:@"profile"] withOverrideParams:nil delegate:self];
    }
    else if (apiResult.api == kAPIGetAccessToken) {
        credentialsProvider.logins = @{ @(AWSCognitoLoginProviderKeyLoginWithAmazon): apiResult.result };
    }
}}
```

## Use Login with Amazon: iOS \- Swift<a name="set-up-amazon-1.ios-swift"></a>

After you authenticate Amazon login, you can pass the token to the Amazon Cognito credentials provider in the `requestDidSucceed` method of the `AMZNAccessTokenDelegate`:

```
func requestDidSucceed(apiResult: APIResult!) {
    if apiResult.api == API.AuthorizeUser {
        AIMobileLib.getAccessTokenForScopes(["profile"], withOverrideParams: nil, delegate: self)
    } else if apiResult.api == API.GetAccessToken {
        credentialsProvider.logins = [AWSCognitoLoginProviderKey.LoginWithAmazon.rawValue: apiResult.result]
    }
}
```

## Use Login with Amazon: JavaScript<a name="set-up-amazon-1.javascript"></a>

After the user authenticates with Login with Amazon and is redirected back to your website, the Login with Amazon access\_token is provided in the query string\. Pass that token into the credentials login map\.

```
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
   IdentityPoolId: 'IDENTITY_POOL_ID',
   Logins: {
       'www.amazon.com': 'Amazon Access Token'
   }
});
```

## Use Login with Amazon: Xamarin<a name="set-up-amazon-1.xamarin"></a>

**Xamarin for Android**

```
AmazonAuthorizationManager manager = new AmazonAuthorizationManager(this, Bundle.Empty);

var tokenListener = new APIListener {
  Success = response => {
    // Get the auth token
    var token = response.GetString(AuthzConstants.BUNDLE_KEY.Token.Val);
    credentials.AddLogin("www.amazon.com", token);
  }
};

// Try and get existing login
manager.GetToken(new[] {
  "profile"
}, tokenListener);
```

**Xamarin for iOS**

In `AppDelegate.cs`, insert the following:

```
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{
    // Pass on the url to the SDK to parse authorization code from the url
    bool isValidRedirectSignInURL = AIMobileLib.HandleOpenUrl (url, sourceApplication);
    if(!isValidRedirectSignInURL)
        return false;

    // App may also want to handle url
    return true;
}
```

Then, in `ViewController.cs`, do the following:

```
public override void ViewDidLoad ()
{
    base.LoadView ();

    // Here we create the Amazon Login Button
    btnLogin = UIButton.FromType (UIButtonType.RoundedRect);
    btnLogin.Frame = new RectangleF (55, 206, 209, 48);
    btnLogin.SetTitle ("Login using Amazon", UIControlState.Normal);
    btnLogin.TouchUpInside += (sender, e) => {
        AIMobileLib.AuthorizeUser (new [] { "profile"}, new AMZNAuthorizationDelegate ());
    };
    View.AddSubview (btnLogin);
}

// Class that handles Authentication Success/Failure
public class AMZNAuthorizationDelegate : AIAuthenticationDelegate
{
  public override void RequestDidSucceed(ApiResult apiResult)
    {
      // Your code after the user authorizes application for requested scopes
      var token = apiResult["access_token"];
      credentials.AddLogin("www.amazon.com",token);
    }

    public override void RequestDidFail(ApiError errorResponse)
    {
      // Your code when the authorization fails
      InvokeOnMainThread(() => new UIAlertView("User Authorization Failed", errorResponse.Error.Message, null, "Ok", null).Show());
    }
}
```