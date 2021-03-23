# Switching Unauthenticated Users to Authenticated Users \(Identity Pools\)<a name="switching-identities"></a>

Amazon Cognito identity pools support both authenticated and unauthenticated users\. Unauthenticated users receive access to your AWS resources even if they aren't logged in with any of your identity providers \(IdPs\)\. This degree of access is useful to display content to users before they log in\. Each unauthenticated user has a unique identity in the identity pool, even though they haven't been individually logged in and authenticated\.

This section describes the case where your user chooses to switch from logging in with an unauthenticated identity to using an authenticated identity\.

## Android<a name="switching-identities-1.android"></a>

Users can log in to your application as unauthenticated guests\. Eventually they might decide to log in using one of the supported IdPs\. Amazon Cognito makes sure that an old identity retains the same unique identifier as the new one, and that the profile data is merged automatically\.

Your application is informed of a profile merge through the `IdentityChangedListener` interface\. Implement the `identityChanged` method in the interface to receive these messages:

```
@override
public void identityChanged(String oldIdentityId, String newIdentityId) {
    // handle the change
}
```

## iOS \- Objective\-C<a name="switching-identities-1.ios-objc"></a>

Users can log in to your application as unauthenticated guests\. Eventually they might decide to log in using one of the supported IdPs\. Amazon Cognito makes sure that an old identity retains the same unique identifier as the new one, and that the profile data is merged automatically\.

`NSNotificationCenter` informs your application of a profile merge:

```
[[NSNotificationCenter defaultCenter] addObserver:self
                                      selector:@selector(identityIdDidChange:)
                                      name:AWSCognitoIdentityIdChangedNotification
                                      object:nil];

-(void)identityDidChange:(NSNotification*)notification {
    NSDictionary *userInfo = notification.userInfo;
    NSLog(@"identity changed from %@ to %@",
        [userInfo objectForKey:AWSCognitoNotificationPreviousId],
        [userInfo objectForKey:AWSCognitoNotificationNewId]);
}
```

## iOS \- Swift<a name="switching-identities-1.ios-swift"></a>

Users can log in to your application as unauthenticated guests\. Eventually they might decide to log in using one of the supported IdPs\. Amazon Cognito makes sure that an old identity retains the same unique identifier as the new one, and that the profile data is merged automatically\.

`NSNotificationCenter` informs your application of a profile merge:

```
[NSNotificationCenter.defaultCenter().addObserver(observer: self
   selector:"identityDidChange"
   name:AWSCognitoIdentityIdChangedNotification
   object:nil)

func identityDidChange(notification: NSNotification!) {
  if let userInfo = notification.userInfo as? [String: AnyObject] {
    print("identity changed from: \(userInfo[AWSCognitoNotificationPreviousId])
    to: \(userInfo[AWSCognitoNotificationNewId])")
  }
}
```

## JavaScript<a name="switching-identities-1.javascript"></a>

### Initially Unauthenticated User<a name="switching-identities-1.javascript-unauth"></a>

Users typically start with the unauthenticated role\. For this role, you set the credentials property of your configuration object without a Logins property\. In this case, your default configuration might look like the following:

```
// set the default config object
var creds = new AWS.CognitoIdentityCredentials({
    IdentityPoolId: 'us-east-1:1699ebc0-7900-4099-b910-2df94f52a030'
});
AWS.config.credentials = creds;
```

### Switch to Authenticated User<a name="switching-identities-1.javascript-auth"></a>

When an unauthenticated user logs in to an IdP and you have a token, you can switch the user from unauthenticated to authenticated by calling a custom function that updates the credentials object and adds the Logins token:

```
// Called when an identity provider has a token for a logged in user
function userLoggedIn(providerName, token) {
    creds.params.Logins = creds.params.Logins || {};
    creds.params.Logins[providerName] = token;

    // Expire credentials to refresh them on the next request
    creds.expired = true;
}
```

You can also create a `CognitoIdentityCredentials` object\. If you do, you must reset the credentials properties of any existing service objects to reflect the updated credentials configuration information\. See [Using the Global Configuration Object](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/global-config-object.html)\.

For more information about the `CognitoIdentityCredentials` object, see [AWS\.CognitoIdentityCredentials](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CognitoIdentityCredentials.html) in the AWS SDK for JavaScript API Reference\.

## Unity<a name="switching-identities-1.unity"></a>

Users can log in to your application as unauthenticated guests\. Eventually they might decide to log in using one of the supported IdPs\. Amazon Cognito makes sure that an old identity retains the same unique identifier as the new one, and that the profile data is merged automatically\.

You can subscribe to the `IdentityChangedEvent` to be notified of profile merges:

```
credentialsProvider.IdentityChangedEvent += delegate(object sender, CognitoAWSCredentials.IdentityChangedArgs e)
{
    // handle the change
    Debug.log("Identity changed from " + e.OldIdentityId + " to " + e.NewIdentityId);
};
```

## Xamarin<a name="switching-identities-1.xamarin"></a>

Users can log in to your application as unauthenticated guests\. Eventually they might decide to log in using one of the supported IdPs\. Amazon Cognito makes sure that an old identity retains the same unique identifier as the new one, and that the profile data is merged automatically\.

```
credentialsProvider.IdentityChangedEvent += delegate(object sender, CognitoAWSCredentials.IdentityChangedArgs e){
    // handle the change
    Console.WriteLine("Identity changed from " + e.OldIdentityId + " to " + e.NewIdentityId);
};
```