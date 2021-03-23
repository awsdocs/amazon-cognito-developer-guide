# Login with Amazon \(Identity Pools\)<a name="amazon"></a>

Amazon Cognito integrates with Login with Amazon to provide federated authentication for your mobile application and web application users\. This section explains how to register and set up your application using Login with Amazon as an identity provider\.

There are two ways to set up Login with Amazon to work with Amazon Cognito\. If you're not sure which one to use, or if you need to use both, see "Setting Up Login with Amazon" in the [Login with Amazon FAQ](https://developer.amazon.com/public/apis/engage/login-with-amazon/docs/faq.html#Setting%20up%20Login%20with%20Amazon)\.
+ Through the [Amazon Developer Portal](https://developer.amazon.com/login-with-amazon)\. Use this method if you want to let your end users authenticate with Login with Amazon, but you don’t have a Seller Central account\.
+ Through Seller Central using [http://login\.amazon\.com/](http://login.amazon.com/)\. Use this method if you are a retail merchant that uses Seller Central\.

**Note**  
For Xamarin, follow the [Xamarin Getting Started Guide](https://developer.xamarin.com/guides/cross-platform/getting_started/) to integrate Login with Amazon into your Xamarin application\.

**Note**  
Login with Amazon integration is not natively supported on the Unity platform\. Integration currently requires the use of a web view to go through the browser sign\-in flow\.

## Setting Up Login with Amazon<a name="login-with-amazon-setup"></a>

To implement Login with Amazon, do one of the following:
+ Create a Security Profile ID for your application through the [Amazon Developer Portal](https://developer.amazon.com/login-with-amazon)\. Use this method if you want to let your end users authenticate with Amazon, but you don’t have a Seller Central account\. The Developer Portal [Login with Amazon](https://developer.amazon.com/public/apis/engage/login-with-amazon/content/documentation.html) documentation takes you through the process of setting up Login with Amazon in your application, downloading the client SDK, and declaring your application on the Amazon developer platform\. Make a note of the Security Profile ID, as you'll need to enter it as the Amazon App ID when you create an Amazon Cognito identity pool, as described in [Getting Credentials](getting-credentials.md)\.
+ Create an Application ID for your application through Seller Central using [http://login\.amazon\.com/](http://login.amazon.com/)\. Use this method if you are a retail merchant that uses Seller Central\. The Seller Central [Login with Amazon](http://login.amazon.com/documentation) documentation takes you through the process of setting up Login with Amazon in your application, downloading the client SDK, and declaring your application on the Amazon developer platform\. Make a note of the Application ID, as you'll need to enter it as the Amazon App ID when you create an Amazon Cognito identity pool, as described in [Getting Credentials](getting-credentials.html)\.

## Configure the External Provider in the Amazon Cognito Console<a name="login-with-amazon-configure-provider"></a>

Choose **Manage Identity Pools** from the [Amazon Cognito console home page](https://console.aws.amazon.com/cognito/home):

1. Choose the name of the identity pool for which you want to enable Login with Amazon as an external provider\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, choose **Edit identity pool**\. The Edit identity pool page appears\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Amazon** tab\.

1. Choose **Unlock**\.

1. Enter the Amazon App ID you obtained from Amazon, and then choose **Save Changes**\.

## Use Login with Amazon: Android<a name="set-up-amazon-1.android"></a>

Once you've implemented Amazon login, you can pass the token to the Amazon Cognito credentials provider in the onSuccess method of the TokenListener interface\. The code looks like this:

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

Once you've implemented Amazon login, you can pass the token to the Amazon Cognito credentials provider in the requestDidSucceed method of the AMZNAccessTokenDelegate:

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

Once you've implemented Amazon login, you can pass the token to the Amazon Cognito credentials provider in the `requestDidSucceed` method of the `AMZNAccessTokenDelegate`:

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