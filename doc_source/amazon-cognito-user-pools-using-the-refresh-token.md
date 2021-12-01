# Using the refresh token<a name="amazon-cognito-user-pools-using-the-refresh-token"></a>

You can use the refresh token to retrieve new ID and access tokens\. By default, the refresh token expires 30 days after your application user signs into your user pool\. When you create an application for your user pool, you can set the application's refresh token expiration to any value between 60 minutes and 10 years\. 

The Mobile SDK for iOS, Mobile SDK for Android, Amplify for iOS, Android, and Flutter automatically refresh your ID and access tokens if a valid \(unexpired\) refresh token is present\. The ID and access tokens have a minimum remaining validity of 2 minutes\. If the refresh token is expired, your app user must re\-authenticate by signing in again to your user pool\. If the minimum for the access token and ID token is set to 5 minutes, and you are using the SDK, the refresh token will be continually used to retrieve new access and ID tokens\. You will see expected behavior with a minimum of 7 minutes instead of 5 minutes\.

**Note**  
The Mobile SDK for Android offers the option to change the minimum validity period of the ID and access tokens to a value between 0 and 30 minutes\. See the `setRefreshThreshold()` method of [CognitoIdentityProviderClientConfig](https://docs.aws.amazon.com/AWSAndroidSDK/latest/javadoc/com/amazonaws/mobileconnectors/cognitoidentityprovider/util/CognitoIdentityProviderClientConfig.html) in the AWS Mobile SDK for Android API Reference\.

Your user's account itself never expires, as long as the user has logged in at least once before the `UnusedAccountValidityDays` time limit for new accounts\.

## Initiate new refresh tokens \(API\)<a name="amazon-cognito-user-pools-using-the-refresh-token_initiate-token"></a>

You must use the API or hostedUI to initiate authentication for refresh tokens\.

To use the refresh token to get new ID and access tokens with the user pool API, use the [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) or [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) methods\. Pass `REFRESH_TOKEN_AUTH` for the `AuthFlow` parameter\. The authorization parameter, `AuthParameters`, is a key\-value map where the key is `"REFRESH_TOKEN"` and the value is the actual refresh token\. Amazon Cognito responds with new ID and access tokens\.

**Note**  
To change tokens for users signed in using hostedUI, use the `InitiateAuth` API operation\.

## Revoking refresh tokens<a name="amazon-cognito-identity-user-pools-revoking-all-tokens-for-user"></a>

You can ``revoke refresh tokens that belong to a user\. For more information about revoking tokens, see [Revoking tokens](token-revocation.md)\.  

**Note**  
Revoking the refresh token will revoke all tokens which are issued with the refresh token\.

Users can sign out from all devices where they are currently signed in when you revoke all of the user's tokens by using the `GlobalSignOut` and `AdminUserGlobalSignOut` API operations\. After the user is signed out, the following things occur:
+ The user's refresh token cannot be used to get new tokens for the user\.
+ The user's access token cannot be used for the user pools service\.
+ The user must re\-authenticate to get new tokens\. The session cookies do not expire automatically\. As a best practice, applications should redirect users to the logout endpoint to force the browser to clear session cookies\.

An application can use the `GlobalSignOut` API to allow individual users to sign themselves out from all devices\. Typically an application would present this option as a choice, such as **Sign out from all devices**\. The application must call this method with the user's valid, non\-expired, non\-revoked access token\. This method cannot be used to allow a user to sign out another user\.

An application can use the `AdminUserGlobalSignOut` API to allow administrators to sign out a user from all devices\. The administrator application must call this method with AWS developer credentials and pass the user pool ID and the user's user name as parameters\. The `AdminUserGlobalSignOut` API can sign out any user in the user pool\. 