# Sign in with Apple \(Identity Pools\)<a name="apple"></a>

Amazon Cognito integrates with Sign in with Apple to provide federated authentication for your mobile application and web application users\. This section explains how to register and set up your application using Sign in with Apple as an identity provider\.

Adding Sign in with Apple as an authentication provider to an identity pool is a two\-step process\. First you integrate Sign in with Apple in an application, and then you configure Sign in with Apple in identity pools\. 

## Set Up Sign in with Apple<a name="login-with-apple-setup"></a>

To configure Sign in with Apple as an identity provider, you must register your application with the Apple to receive client ID\.

1. Create a [developer account with Apple](https://developer.apple.com/programs/enroll/)\.

1. [Sign in](https://developer.apple.com/account/#/welcome) with your Apple credentials\.

1. In the left navigation pane, choose **Certificates, IDs & Profiles**\.

1. In the left navigation pane, choose **Identifiers**\.

1. On the **Identifiers** page, choose the **\+**icon\.

1. On the **Register a New Identifier** page, choose **App IDs** and then choose **Continue**\.

1. On the **Register an App ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **Bundle ID,** type an identifier\. Make a note of this bundle ID as you will need this value to configure Apple as a provider in the identity pool\. 

   1. Under **Capabilities**, choose **Sign In with Apple** and then choose **Edit**\.

   1. On the **Sign in with Apple: App ID Configuration** page, select the appropriate setting for you app, Then choose **Save**\.

   1. Choose **Continue**\.

1. On the **Confirm your App ID** page, choose **Register**\.

1. Proceed to step 10 if you want to integrate Sign in with Apple with a native iOS application\. Step 11 is for applications that you want to integrate with Sign in with Apple JS\.

1. On the **Identifiers** page, pause on **App IDs** on the right side of the page\. Choose **Services IDs** and then choose the **plus \+** icon\.

1. On the **Register a New Identifier** page, choose **Services IDs** and then choose **Continue**\.

1. On the **Register a Services ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **Identifier**, type an identifier\. Make a note of the services ID as you will need this value to configure Apple as a provider in your identity pool\. 

   1. Select **Sign In with Apple** and then choose **Configure**\.

   1. On the **Web Authentication Configuration** page, choose a **Primary App ID**\. Under **Website URL’s**, choose the **\+ **icon\. For **Domains and Subdomains**, enter the domain name of your app\. In **Return URLs,** enter the callback URL that the authorization redirects to after Sign in with Apple authentication\. 

   1. Choose **Next**\.

   1. Choose **Continue** and then choose **Register**\.

1. In the left navigation pane, choose **Keys**\.

1. On the **Keys** page, choose the **\+** icon\.

1. On the **Register a New Key** page, do the following:

   1. Under **Key Name**, type a key name\. 

   1. Choose **Sign In with Apple** and then choose **Configure**\.

   1. On the **Configure Key** page, choose a **Primary App ID** and then choose **Save**\.

   1. Choose **Continue** and then choose **Register**\.

**Note**  
To integrate Sign in with Apple with a native iOS application, see [ Implementing User Authentication with Sign in with Apple\. ](https://developer.apple.com/documentation/authenticationservices/implementing_user_authentication_with_sign_in_with_apple)  
To integrate Sign in with Apple in a platform other than native iOS, see [ Sign in with Apple JS\.](https://developer.apple.com/documentation/sign_in_with_apple/sign_in_with_apple_js) 

## Configure the External provider in the Amazon Cognito Federated Identities Console<a name="login-with-apple-configure-provider"></a>

Use the following procedure to configure your external provider\.

1. Choose **Manage Identity Pools** from the [Amazon Cognito console home page](https://console.aws.amazon.com/cognito/home)\.

1. Choose the name of the identity pool for which you want to enable Apple as an external provider\.

1. In the top\-right corner of the dashboard, choose **Edit identity pool**\.

1. Scroll down and choose **Authentication providers** to expand it\.

1. Choose the **Apple** tab\.

1. Enter the  Bundle ID that you obtained from https://developer\.apple\.com\. Then choose **Save Changes\.**

1. If you use Sign in with Apple with native iOS applications, enter the `BundleID` you obtained from developer\.apple\.com\. Or if you use Sign in with Apple with web or other applications, enter the service ID\. Then choose **Save Changes**\.

## Sign in with Apple as a Provider in the Amazon Cognito Federated Identities CLI Examples<a name="sign-in-with-apple-cli-examples"></a>

This example creates an identity pool named `MyIdentityPool` with Sign in with Apple as an identity provider\.

`aws cognito-identity create-identity-pool --identity-pool-name MyIdentityPool --supported-login-providers appleid.apple.com="sameple.apple.clientid"`

 For more information, see [Create identity pool]( https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/create-identity-pool.html) 

**Generate an Amazon Cognito identity ID**  
 This example generates \(or retrieves\) an Amazon Cognito ID\. This is a public API so you don't need any credentials to call this API\.

`aws cognito-identity get-id --identity-pool-id SampleIdentityPoolId --logins appleid.apple.com="SignInWithAppleIdToken"`

For more information, see [get\-id\.](https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/get-id.html) 

**Get credentials for an Amazon Cognito identity ID**  
This example returns credentials for the provided identity ID and Sign in with Apple login\. This is a public API so you don't need any credentials to call this API\.

`aws cognito-identity get-credentials-for-identity --identity-id SampleIdentityId --logins appleid.apple.com="SignInWithAppleIdToken" `

For more information, see [get\-credentials\-for\-identity](https://docs.aws.amazon.com/cli/latest/reference/cognito-identity/get-credentials-for-identity.html) 

## Use Sign in with Apple: Android<a name="set-up-apple-1.android"></a>

Apple doesn't provide an SDK that supports Sign in with Apple for Android\. You can use the web flow in a web view instead\.
+ To configure Sign in with Apple in your application, follow [Configuring Your Web page for Sign In with Apple](https://developer.apple.com/documentation/signinwithapplejs/configuring_your_webpage_for_sign_in_with_apple) in the Apple documentation\.
+ To add a **Sign in with Apple** button to your Android user interface, follow [Displaying and Configuring Sign In with Apple Buttons](https://developer.apple.com/documentation/signinwithapplejs/displaying_and_configuring_sign_in_with_apple_buttons) in the Apple documentation\.
+ To securely authenticate users using Sign in with Apple, follow [Configuring Your webpage for Sign In with Apple](https://developer.apple.com/documentation/signinwithapplerestapi/authenticating_users_with_sign_in_with_apple) in the Apple documentation\.

Sign in with Apple uses a session object to track its state\. Amazon Cognito uses the ID token from this session object to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\.

```
@Override
public void onSuccess(Bundle response) {
    String token = response.getString("id_token");
    Map<String, String> logins = new HashMap<String, String>();
    logins.put("appleid.apple.com", token);
    credentialsProvider.setLogins(logins);
}
```

## Use Sign in with Apple: iOS \- Objective\-C<a name="set-up-apple-1.ios-objc"></a>

Apple provided SDK support for Sign in with Apple in native iOS applications\. To implement user authentication with Sign in with Apple in native iOS devices, follow [Implementing User Authentication with Sign in with Apple](https://developer.apple.com/documentation/authenticationservices/implementing_user_authentication_with_sign_in_with_apple) in the Apple documentation\.

Amazon Cognito uses the ID token to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\.

```
(void)finishedWithAuth: (ASAuthorizationAppleIDCredential *)auth error: (NSError *) error {
        NSString *idToken = [ASAuthorizationAppleIDCredential objectForKey:@"identityToken"];
        credentialsProvider.logins = @{ "appleid.apple.com": idToken };
    }
```

## Use Sign in with Apple: iOS \- Swift<a name="set-up-apple-1.ios-swift"></a>

Apple provided SDK support for Sign in with Apple in native iOS applications\. To implement user authentication with Sign in with Apple in native iOS devices, follow [Implementing User Authentication with Sign in with Apple](https://developer.apple.com/documentation/authenticationservices/implementing_user_authentication_with_sign_in_with_apple) in the Apple documentation\.

Amazon Cognito uses the ID token to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\.

For more information on setting up Sign in with Apple in iOS, see [Set up Sign in with Apple](https://docs.amplify.aws/sdk/auth/federated-identities/q/platform/ios#set-up-sign-in-with-apple)

```
func finishedWithAuth(auth: ASAuthorizationAppleIDCredential!, error: NSError!) {
    if error != nil {
      print(error.localizedDescription)
    }
    else {
      let idToken = auth.identityToken,
      credentialsProvider.logins = ["appleid.apple.com": idToken!]
    }
}
```

## Use Sign in with Apple: JavaScript<a name="set-up-apple-1.javascript"></a>

Apple doesn’t provide an SDK that supports Sign in with Apple for JavaScript\. You can use the web flow in a web view instead\.
+ To configure Sign in with Apple in your application, follow [Configuring Your Web page for Sign In with Apple](https://developer.apple.com/documentation/signinwithapplejs/configuring_your_webpage_for_sign_in_with_apple) in the Apple documentation\.
+ To add a **Sign in with Apple** button to your JavaScript user interface, follow [Displaying and Configuring Sign In with Apple Buttons](https://developer.apple.com/documentation/signinwithapplejs/displaying_and_configuring_sign_in_with_apple_buttons) in the Apple documentation\.
+ To securely authenticate users using Sign in with Apple, follow [Configuring Your Web page for Sign In with Apple](https://developer.apple.com/documentation/signinwithapplerestapi/authenticating_users_with_sign_in_with_apple) in the Apple documentation\.

Sign in with Apple uses a session object to track its state\. Amazon Cognito uses the ID token from this session object to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\.

```
function signinCallback(authResult) {
     // Add the apple's id token to the Amazon Cognito credentials login map.
     AWS.config.credentials = new AWS.CognitoIdentityCredentials({
        IdentityPoolId: 'IDENTITY_POOL_ID',
        Logins: {
           'appleid.apple.com': authResult['id_token']
        }
     });

     // Obtain AWS credentials
     AWS.config.credentials.get(function(){
        // Access AWS resources here.
     });
}
```

## Use Sign in with Apple: Xamarin<a name="set-up-apple-1.xamarin"></a>

We don’t have an SDK that supports Sign in with Apple for Xamarin\. You can use the web flow in a web view instead\.
+ To configure Sign in with Apple in your application, follow [Configuring Your Web page for Sign In with Apple](https://developer.apple.com/documentation/signinwithapplejs/configuring_your_webpage_for_sign_in_with_apple) in the Apple documentation\.
+ To add a **Sign in with Apple** button to your Xamarin user interface, follow [Displaying and Configuring Sign In with Apple Buttons](https://developer.apple.com/documentation/signinwithapplejs/displaying_and_configuring_sign_in_with_apple_buttons) in the Apple documentation\.
+ To securely authenticate users using Sign in with Apple, follow [Configuring Your Web page for Sign In with Apple](https://developer.apple.com/documentation/signinwithapplerestapi/authenticating_users_with_sign_in_with_apple) in the Apple documentation\.

Sign in with Apple uses a session object to track its state\. Amazon Cognito uses the ID token from this session object to authenticate the user, generate the unique identifier, and, if needed, grant the user access to other AWS resources\.

Once you have the token, you can set it in your `CognitoAWSCredentials`:

```
credentials.AddLogin("appleid.apple.com", token);
```