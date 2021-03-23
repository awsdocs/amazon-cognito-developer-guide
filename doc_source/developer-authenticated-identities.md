# Developer Authenticated Identities \(Identity Pools\)<a name="developer-authenticated-identities"></a>

Amazon Cognito supports developer authenticated identities, in addition to web identity federation through [Facebook \(Identity Pools\)](facebook.md), [Google \(Identity Pools\)](google.md), [Login with Amazon \(Identity Pools\)](amazon.md), and [Sign in with Apple \(Identity Pools\)](apple.md)\. With developer authenticated identities, you can register and authenticate users via your own existing authentication process, while still using Amazon Cognito to synchronize user data and access AWS resources\. Using developer authenticated identities involves interaction between the end user device, your backend for authentication, and Amazon Cognito\. For more details, please read our [blog](http://mobile.awsblog.com/post/Tx2FL1QAPDE0UAH/Understanding-Amazon-Cognito-Authentication-Part-2-Developer-Authenticated-Ident)\.

## Understanding the Authentication Flow<a name="understanding-the-authentication-flow"></a>

For information on the developer authenticated identities authflow and how it differs from the external provider authflow, see [Identity Pools \(Federated Identities\) Authentication Flow](authentication-flow.md)\.

## Define a Developer Provider Name and Associate it with an Identity Pool<a name="associate-developer-provider"></a>

To use developer authenticated identities, you'll need an identity pool associated with your developer provider\. To do so, follow these steps:

1. Log in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. Create a new identity pool and, as part of the process, define a developer provider name in the **Custom** tab in **Authentication Providers**\.

1. Alternatively, edit an existing identity pool and define a developer provider name in the **Custom** tab in **Authentication Providers**\.

Note: Once the provider name has been set, it cannot be changed\.

For additional instructions on working with the Amazon Cognito Console, see [Using the Amazon Cognito Console](cognito-console.md)\.

## Implement an Identity Provider<a name="implement-an-identity-provider"></a>

### Android<a name="implement-id-provider-1.android"></a>

To use developer authenticated identities, implement your own identity provider class which extends `AWSAbstractCognitoIdentityProvider`\. Your identity provider class should return a response object containing the token as an attribute\.

Below is a simple example of an identity provider\.

```
public class DeveloperAuthenticationProvider extends AWSAbstractCognitoDeveloperIdentityProvider {

  private static final String developerProvider = "<Developer_provider_name>";

  public DeveloperAuthenticationProvider(String accountId, String identityPoolId, Regions region) {
    super(accountId, identityPoolId, region);
    // Initialize any other objects needed here.
  }

  // Return the developer provider name which you choose while setting up the
  // identity pool in the &COG; Console

  @Override
  public String getProviderName() {
    return developerProvider;
  }

  // Use the refresh method to communicate with your backend to get an
  // identityId and token.

  @Override
  public String refresh() {

    // Override the existing token
    setToken(null);

    // Get the identityId and token by making a call to your backend
    // (Call to your backend)

    // Call the update method with updated identityId and token to make sure
    // these are ready to be used from Credentials Provider.

    update(identityId, token);
    return token;

  }

  // If the app has a valid identityId return it, otherwise get a valid
  // identityId from your backend.

  @Override
  public String getIdentityId() {

    // Load the identityId from the cache
    identityId = cachedIdentityId;

    if (identityId == null) {
       // Call to your backend
    } else {
       return identityId;
    }

  }
}
```

To use this identity provider, you have to pass it into `CognitoCachingCredentialsProvider`\. Here's an example:

```
DeveloperAuthenticationProvider developerProvider = new DeveloperAuthenticationProvider( null, "IDENTITYPOOLID", context, Regions.USEAST1);
CognitoCachingCredentialsProvider credentialsProvider = new CognitoCachingCredentialsProvider( context, developerProvider, Regions.USEAST1);
```

### iOS \- Objective\-C<a name="implement-id-provider-1.ios-objc"></a>

To use developer authenticated identities, implement your own identity provider class which extends [AWSCognitoCredentialsProviderHelper](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProviderHelper.html)\. Your identity provider class should return a response object containing the token as an attribute\.

```
@implementation DeveloperAuthenticatedIdentityProvider
/*
 * Use the token method to communicate with your backend to get an
 * identityId and token.
 */

- (AWSTask <NSString*> *) token {
    //Write code to call your backend:
    //Pass username/password to backend or some sort of token to authenticate user
    //If successful, from backend call getOpenIdTokenForDeveloperIdentity with logins map 
    //containing "your.provider.name":"enduser.username"
    //Return the identity id and token to client
    //You can use AWSTaskCompletionSource to do this asynchronously

    // Set the identity id and return the token
    self.identityId = response.identityId;
    return [AWSTask taskWithResult:response.token];
}

@end
```

To use this identity provider, pass it into `AWSCognitoCredentialsProvider` as shown in the following example:

```
DeveloperAuthenticatedIdentityProvider * devAuth = [[DeveloperAuthenticatedIdentityProvider alloc] initWithRegionType:AWSRegionYOUR_IDENTITY_POOL_REGION 
                                         identityPoolId:@"YOUR_IDENTITY_POOL_ID"
                                        useEnhancedFlow:YES
                                identityProviderManager:nil];
AWSCognitoCredentialsProvider *credentialsProvider = [[AWSCognitoCredentialsProvider alloc]
                                                          initWithRegionType:AWSRegionYOUR_IDENTITY_POOL_REGION
                                                          identityProvider:devAuth];
```

If you want to support both unauthenticated identities and developer authenticated identities, override the `logins` method in your `AWSCognitoCredentialsProviderHelper` implementation\.

```
- (AWSTask<NSDictionary<NSString *, NSString *> *> *)logins {
    if(/*logic to determine if user is unauthenticated*/) {
        return [AWSTask taskWithResult:nil];
    }else{
        return [super logins];
    }
}
```

If you want to support developer authenticated identities and social providers, you must manage who the current provider is in your `logins` implementation of `AWSCognitoCredentialsProviderHelper`\.

```
- (AWSTask<NSDictionary<NSString *, NSString *> *> *)logins {
    if(/*logic to determine if user is unauthenticated*/) {
        return [AWSTask taskWithResult:nil];
    }else if (/*logic to determine if user is Facebook*/){
        return [AWSTask taskWithResult: @{ AWSIdentityProviderFacebook : [FBSDKAccessToken currentAccessToken] }];
    }else {
        return [super logins];
    }
}
```

### iOS \- Swift<a name="implement-id-provider-1.ios-swift"></a>

To use developer authenticated identities, implement your own identity provider class which extends [AWSCognitoCredentialsProviderHelper](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProviderHelper.html)\. Your identity provider class should return a response object containing the token as an attribute\.

```
import AWSCore
/*
 * Use the token method to communicate with your backend to get an
 * identityId and token.
 */
class DeveloperAuthenticatedIdentityProvider : AWSCognitoCredentialsProviderHelper {
    override func token() -> AWSTask<NSString> {
    //Write code to call your backend:
    //pass username/password to backend or some sort of token to authenticate user, if successful, 
    //from backend call getOpenIdTokenForDeveloperIdentity with logins map containing "your.provider.name":"enduser.username"
    //return the identity id and token to client
    //You can use AWSTaskCompletionSource to do this asynchronously

    // Set the identity id and return the token
    self.identityId = resultFromAbove.identityId
    return AWSTask(result: resultFromAbove.token)
}
```

To use this identity provider, pass it into `AWSCognitoCredentialsProvider` as shown in the following example:

```
let devAuth = DeveloperAuthenticatedIdentityProvider(regionType: .YOUR_IDENTITY_POOL_REGION, identityPoolId: "YOUR_IDENTITY_POOL_ID", useEnhancedFlow: true, identityProviderManager:nil)
let credentialsProvider = AWSCognitoCredentialsProvider(regionType: .YOUR_IDENTITY_POOL_REGION, identityProvider:devAuth)
let configuration = AWSServiceConfiguration(region: .YOUR_IDENTITY_POOL_REGION, credentialsProvider:credentialsProvider)
AWSServiceManager.default().defaultServiceConfiguration = configuration
```

If you want to support both unauthenticated identities and developer authenticated identities, override the `logins` method in your `AWSCognitoCredentialsProviderHelper` implementation\.

```
override func logins () -> AWSTask<NSDictionary> {
    if(/*logic to determine if user is unauthenticated*/) {
        return AWSTask(result:nil)
    }else {
        return super.logins()
    }
}
```

If you want to support developer authenticated identities and social providers, you must manage who the current provider is in your `logins` implementation of `AWSCognitoCredentialsProviderHelper`\.

```
override func logins () -> AWSTask<NSDictionary> {
    if(/*logic to determine if user is unauthenticated*/) {
        return AWSTask(result:nil)
    }else if (/*logic to determine if user is Facebook*/){
        if let token = AccessToken.current?.authenticationToken {
            return AWSTask(result: [AWSIdentityProviderFacebook:token])
        }
        return AWSTask(error:NSError(domain: "Facebook Login", code: -1 , userInfo: ["Facebook" : "No current Facebook access token"]))
    }else {
        return super.logins()
    }
}
```

### JavaScript<a name="implement-id-provider-1.javascript"></a>

Once you obtain an identity ID and session token from your backend, you will to pass them into the `AWS.CognitoIdentityCredentials` provider\. Here's an example:

```
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
   IdentityPoolId: 'IDENTITY_POOL_ID',
   IdentityId: 'IDENTITY_ID_RETURNED_FROM_YOUR_PROVIDER',
   Logins: {
      'cognito-identity.amazonaws.com': 'TOKEN_RETURNED_FROM_YOUR_PROVIDER'
   }
});
```

### Unity<a name="implement-id-provider-1.unity"></a>

To use developer\-authenticated identities you have to extend `CognitoAWSCredentials` and override the `RefreshIdentity` method to retrieve the user identity id and token from your backend and return them\. Below is a simple example of an identity provider that would contact a hypothetical backend at 'example\.com':

```
using UnityEngine;
using System.Collections;
using Amazon.CognitoIdentity;
using System.Collections.Generic;
using ThirdParty.Json.LitJson;
using System;
using System.Threading;

public class DeveloperAuthenticatedCredentials : CognitoAWSCredentials
{
    const string PROVIDER_NAME = "example.com";
    const string IDENTITY_POOL = "IDENTITY_POOL_ID";
    static readonly RegionEndpoint REGION = RegionEndpoint.USEast1;

    private string login = null;

    public DeveloperAuthenticatedCredentials(string loginAlias)
        : base(IDENTITY_POOL, REGION)
    {
        login = loginAlias;
    }

    protected override IdentityState RefreshIdentity()
    {
        IdentityState state = null;
        ManualResetEvent waitLock = new ManualResetEvent(false);
        MainThreadDispatcher.ExecuteCoroutineOnMainThread(ContactProvider((s) =>
        {
            state = s;
            waitLock.Set();
        }));
        waitLock.WaitOne();
        return state;
    }

    IEnumerator ContactProvider(Action<IdentityState> callback)
    {
        WWW www = new WWW("http://example.com/?username="+login);
        yield return www;
        string response = www.text;

        JsonData json = JsonMapper.ToObject(response);

        //The backend has to send us back an Identity and a OpenID token
        string identityId = json["IdentityId"].ToString();
        string token = json["Token"].ToString();

        IdentityState state = new IdentityState(identityId, PROVIDER_NAME, token, false);
        callback(state);
    }
}
```

The code above uses a thread dispatcher object to call a coroutine\. If you don't have a way to do this in your project, you can use the following script in your scenes:

```
using System;
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class MainThreadDispatcher : MonoBehaviour
{
    static Queue<IEnumerator> _coroutineQueue = new Queue<IEnumerator>();
    static object _lock = new object();

    public void Update()
    {
        while (_coroutineQueue.Count > 0)
        {
            StartCoroutine(_coroutineQueue.Dequeue());
        }
    }

    public static void ExecuteCoroutineOnMainThread(IEnumerator coroutine)
    {
        lock (_lock) {
            _coroutineQueue.Enqueue(coroutine);
        }
    }
}
```

### Xamarin<a name="implement-id-provider-1.xamarin"></a>

To use developer\-authenticated identities you have to extend `CognitoAWSCredentials` and override the `RefreshIdentity` method to retrieve the user identity id and token from your backend and return them\. Below is a simple example of an identity provider that would contact a hypothetical backend at 'example\.com':

```
public class DeveloperAuthenticatedCredentials : CognitoAWSCredentials
{
    const string PROVIDER_NAME = "example.com";
    const string IDENTITY_POOL = "IDENTITY_POOL_ID";
    static readonly RegionEndpoint REGION = RegionEndpoint.USEast1;
    private string login = null;

    public DeveloperAuthenticatedCredentials(string loginAlias)
        : base(IDENTITY_POOL, REGION)
    {
        login = loginAlias;
    }

    protected override async Task<IdentityState> RefreshIdentityAsync()
    {
        IdentityState state = null;
        //get your identity and set the state
        return state;
    }
}
```

## Updating the Logins Map \(Android and iOS only\)<a name="updating-the-logins-map"></a>

### Android<a name="updating-logins-map-1.android"></a>

After successfully authenticating the user with your authentication system, update the logins map with the developer provider name and a developer user identifier, which is an alphanumeric string that uniquely identifies a user in your authentication system\. Be sure to call the `refresh` method after updating the logins map as the `identityId` might have changed:

```
HashMap<String, String> loginsMap = new HashMap<String, String>();
loginsMap.put(developerAuthenticationProvider.getProviderName(), developerUserIdentifier);

credentialsProvider.setLogins(loginsMap);
credentialsProvider.refresh();
```

### iOS \- Objective\-C<a name="updating-logins-map-1.ios-objc"></a>

The iOS SDK only calls your `logins` method to get the latest logins map if there are no credentials or they have expired\. If you want to force the SDK to obtain new credentials \(e\.g\., your end user went from unauthenticated to authenticated and you want credentials against the authenticated user\), call `clearCredentials` on your `credentialsProvider`\.

```
[credentialsProvider clearCredentials];
```

### iOS \- Swift<a name="updating-logins-map-1.ios-swift"></a>

The iOS SDK only calls your `logins` method to get the latest logins map if there are no credentials or they have expired\. If you want to force the SDK to obtain new credentials \(e\.g\., your end user went from unauthenticated to authenticated and you want credentials against the authenticated user\), call `clearCredentials` on your `credentialsProvider`\.

```
credentialsProvider.clearCredentials()
```

## Getting a Token \(Server Side\)<a name="getting-a-token-server-side"></a>

You obtain a token by calling [GetOpenIdTokenForDeveloperIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetOpenIdTokenForDeveloperIdentity.html)\. This API must be invoked from your backend using AWS developer credentials\. It must not be invoked from the client SDK\. The API receives the Cognito identity pool ID; a logins map containing your identity provider name as the key and identifier as the value; and optionally a Cognito identity ID \(i\.e\., you are making an unauthenticated user authenticated\)\. The identifier can be the username of your user, an email address, or a numerical value\. The API responds to your call with a unique Cognito ID for your user and an OpenID Connect token for the end user\.

A few things to keep in mind about the token returned by `GetOpenIdTokenForDeveloperIdentity`:
+ You can specify a custom expiration time for the token so you can cache it\. If you don't provide any custom expiration time, the token is valid for 15 minutes\. 
+ The maximum token duration you can set is 24 hours\.
+ Be mindful of the security implications of increasing the token duration\. If an attacker obtains this token, they can exchange it for AWS credentials for the end user for the token duration\.

The following Java snippet shows how to initialize an Amazon Cognito client and retrieve a token for a developer authenticated identity\.

```
// authenticate your end user as appropriate
// ....

// if authenticated, initialize a cognito client with your AWS developer credentials
AmazonCognitoIdentity identityClient = new AmazonCognitoIdentityClient(
  new BasicAWSCredentials("access_key_id", "secret_access_key")
);

// create a new request to retrieve the token for your end user
GetOpenIdTokenForDeveloperIdentityRequest request =
  new GetOpenIdTokenForDeveloperIdentityRequest();
request.setIdentityPoolId("YOUR_COGNITO_IDENTITY_POOL_ID");

request.setIdentityId("YOUR_COGNITO_IDENTITY_ID"); //optional, set this if your client has an
                                                   //identity ID that you want to link to this 
                                                   //developer account

// set up your logins map with the username of your end user
HashMap<String,String> logins = new HashMap<>();
logins.put("YOUR_IDENTITY_PROVIDER_NAME","YOUR_END_USER_IDENTIFIER");
request.setLogins(logins);

// optionally set token duration (in seconds)
request.setTokenDuration(60 * 15l);
GetOpenIdTokenForDeveloperIdentityResult response =
  identityClient.getOpenIdTokenForDeveloperIdentity(request);

// obtain identity id and token to return to your client
String identityId = response.getIdentityId();
String token = response.getToken();

//code to return identity id and token to client
//...
```

Following the steps above, you should be able to integrate developer authenticated identities in your app\. If you have any issues or questions please feel free to post in our [forums](https://forums.aws.amazon.com/forum.jspa?forumID=173)\.

## Connect to an Existing Social Identity<a name="connect-to-an-existing-social-identity"></a>

All linking of providers when you are using developer authenticated identities must be done from your backend\. To connect a custom identity to a user's social identity \(Login with Amazon, Sign in with Apple, Facebook, or Google\), add the identity provider token to the logins map when you call [GetOpenIdTokenForDeveloperIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetOpenIdTokenForDeveloperIdentity.html)\. To make this possible, when you call your backend from your client SDK to authenticate your end user, additionally pass the end user's social provider token\.

For example, if you are trying to link a custom identity to Facebook, you would add the Facebook token in addition to your identity provider identifier to the logins map when you call `GetOpenIdTokenForDeveloperIdentity`\.

```
logins.put("YOUR_IDENTITY_PROVIDER_NAME","YOUR_END_USER_IDENTIFIER");
logins.put("graph.facebook.com","END_USERS_FACEBOOK_ACCESSTOKEN");
```

## Supporting Transition Between Providers<a name="supporting-transition-between-providers"></a>

### Android<a name="support-transition-between-providers-1.android"></a>

Your application might require supporting unauthenticated identities or authenticated identities using public providers \(Login with Amazon, Sign in with Apple, Facebook, or Google\) along with developer authenticated identities\. The essential difference between developer authenticated identities and other identities \(unauthenticated identities and authenticated identities using public provider\) is the way the identityId and token are obtained\. For other identities the mobile application will interact directly with Amazon Cognito instead of contacting your authentication system\. So the mobile application should be able to support two distinct flows depending on the choice made by the app user\. For this you will have to make some changes to the custom identity provider\.

The `refresh` method should check the logins map, if the map is not empty and has a key with developer provider name, then you should call your backend; otherwise just call the getIdentityId method and return null\.

```
public String refresh() {

   setToken(null);

   // If the logins map is not empty make a call to your backend
   // to get the token and identityId
   if (getProviderName() != null &&
      !this.loginsMap.isEmpty() &&
      this.loginsMap.containsKey(getProviderName())) {

      /**
       * This is where you would call your backend
       **/

      // now set the returned identity id and token in the provider
      update(identityId, token);
      return token;

   } else {
      // Call getIdentityId method and return null
      this.getIdentityId();
      return null;
   }
}
```

Similarly the `getIdentityId` method will have two flows depending on the contents of the logins map:

```
public String getIdentityId() {

   // Load the identityId from the cache
   identityId = cachedIdentityId;

   if (identityId == null) {

      // If the logins map is not empty make a call to your backend
      // to get the token and identityId

      if (getProviderName() != null && !this.loginsMap.isEmpty()
         && this.loginsMap.containsKey(getProviderName())) {

         /**
           * This is where you would call your backend
          **/

         // now set the returned identity id and token in the provider
         update(identityId, token);
         return token;

      } else {
         // Otherwise call &COG; using getIdentityId of super class
         return super.getIdentityId();
      }

   } else {
      return identityId;
   }

}
```

### iOS \- Objective\-C<a name="support-transition-between-providers-1.ios-objc"></a>

Your application might require supporting unauthenticated identities or authenticated identities using public providers \(Login with Amazon, Sign in with Apple, Facebook, or Google\) along with developer authenticated identities\. To do this, override the [AWSCognitoCredentialsProviderHelper](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProviderHelper.html) `logins` method to be able to return the correct logins map based on the current identity provider\. This example shows you how you might pivot between unauthenticated, Facebook and developer authenticated\.

```
- (AWSTask<NSDictionary<NSString *, NSString *> *> *)logins {
    if(/*logic to determine if user is unauthenticated*/) {
        return [AWSTask taskWithResult:nil];
    }else if (/*logic to determine if user is Facebook*/){
        return [AWSTask taskWithResult: @{ AWSIdentityProviderFacebook : [FBSDKAccessToken currentAccessToken] }];
    }else {
        return [super logins];
    }
}
```

When you transition from unauthenticated to authenticated, you should call `[credentialsProvider clearCredentials];` to force the SDK to get new authenticated credentials\. When you switch between two authenticated providers and you aren't trying to link the two providers \(i\.e\. you are not providing tokens for multiple providers in your logins dictionary\), you should call `[credentialsProvider clearKeychain];`\. This will clear both the credentials and identity and force the SDK to get new ones\.

### iOS \- Swift<a name="support-transition-between-providers-1.ios-swift"></a>

Your application might require supporting unauthenticated identities or authenticated identities using public providers \(Login with Amazon, Sign in with Apple, Facebook, or Google\) along with developer authenticated identities\. To do this, override the [AWSCognitoCredentialsProviderHelper](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoCredentialsProviderHelper.html) `logins` method to be able to return the correct logins map based on the current identity provider\. This example shows you how you might pivot between unauthenticated, Facebook and developer authenticated\.

```
override func logins () -> AWSTask<NSDictionary> {
    if(/*logic to determine if user is unauthenticated*/) {
        return AWSTask(result:nil)
    }else if (/*logic to determine if user is Facebook*/){
        if let token = AccessToken.current?.authenticationToken {
            return AWSTask(result: [AWSIdentityProviderFacebook:token])
        }
        return AWSTask(error:NSError(domain: "Facebook Login", code: -1 , userInfo: ["Facebook" : "No current Facebook access token"]))
    }else {
        return super.logins()
    }
}
```

When you transition from unauthenticated to authenticated, you should call `credentialsProvider.clearCredentials()` to force the SDK to get new authenticated credentials\. When you switch between two authenticated providers and you aren't trying to link the two providers \(i\.e\. you are not providing tokens for multiple providers in your logins dictionary\), you should call `credentialsProvider.clearKeychain()`\. This will clear both the credentials and identity and force the SDK to get new ones\.

### Unity<a name="support-transition-between-providers-1.unity"></a>

Your application might require supporting unauthenticated identities or authenticated identities using public providers \(Login with Amazon, Sign in with Apple, Facebook, or Google\) along with developer authenticated identities\. The essential difference between developer authenticated identities and other identities \(unauthenticated identities and authenticated identities using public provider\) is the way the identityId and token are obtained\. For other identities the mobile application will interact directly with Amazon Cognito instead of contacting your authentication system\. So the mobile application should be able to support two distinct flows depending on the choice made by the app user\. For this you will have to make some changes to the custom identity provider\.

The recommended way to do it in Unity is to extend your identity provider from AmazonCognitoEnhancedIdentityProvide instead of AbstractCognitoIdentityProvider, and call the parent RefreshAsync method instead of your own in case the user is not authenticated with your own backend\. If the user is authenticated, you can use the same flow explained before\.

### Xamarin<a name="support-transition-between-providers-1.xamarin"></a>

Your application might require supporting unauthenticated identities or authenticated identities using public providers \(Login with Amazon, Sign in with Apple, Facebook, or Google\) along with developer authenticated identities\. The essential difference between developer authenticated identities and other identities \(unauthenticated identities and authenticated identities using public provider\) is the way the identityId and token are obtained\. For other identities the mobile application will interact directly with Amazon Cognito instead of contacting your authentication system\. So the mobile application should be able to support two distinct flows depending on the choice made by the app user\. For this you will have to make some changes to the custom identity provider\.