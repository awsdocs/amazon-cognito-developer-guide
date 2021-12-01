# Accessing AWS services using an identity pool after sign\-in<a name="amazon-cognito-integrating-user-pools-with-identity-pools"></a>

You can enable your users to sign\-in with a user pool, and then access AWS services using an identity pool\.

After a successful authentication, your web or mobile app will receive user pool tokens from Amazon Cognito\. You can use those tokens to retrieve AWS credentials that allow your app to access other AWS services\. For more information, see [Getting started with Amazon Cognito identity pools \(federated identities\)](getting-started-with-identity-pools.md)\.

![\[Accessing AWS credentials through a user pool with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing AWS credentials through a user pool with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing AWS credentials through a user pool with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

For more information about using identity pools together with user pool groups to control access your AWS resources see [Adding groups to a user pool](cognito-user-pools-user-groups.md) and [Role\-based access control](role-based-access-control.md)\. See also [Identity pools concepts \(federated identities\) ](concepts.md) for more information about identity pools and AWS Identity and Access Management\.

## Setting up a user pool with the AWS Management Console<a name="amazon-cognito-integrating-user-pools-with-identity-pools-setting-up"></a>

Create an Amazon Cognito user pool and make a note of the **User Pool ID** and **App Client ID** for each of your client apps\. For more information about creating user pools, see [Getting started with user pools](getting-started-with-cognito-user-pools.md)\.

## Setting up an identity pool with the AWS Management Console<a name="amazon-cognito-integrating-user-pools-with-identity-pools-configuring"></a>

The following procedure describes how to use the AWS Management Console to integrate an identity pool with one or more user pools and client apps\.

------
#### [ Original console ]

**To configure your identity pool**

1. Open the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage Identity Pools**\.

1. Choose the name of the identity pool for which you want to enable Amazon Cognito user pools as a provider\.

1. On the **Dashboard** page, choose **Edit identity pool**\.

1. Expand the **Authentication providers** section\.

1. Choose **Cognito**\.

1. Enter the **User Pool ID**\.

1. Enter the **App Client ID**\. This must be the same client app ID that you received when you created the app in the **Your User Pools** section of the AWS Management Console for Amazon Cognito\.

1. If you have additional apps or user pools, choose **Add Another Provider** and enter the **User Pool ID** and **App Client ID** for each app in each user pool\.

1. When you have no more apps or user pools to add, choose **Save Changes**\. If successful, you will see a **Changes saved successfully** message on the **Dashboard** page\.

------
#### [ New console ]

**To configure your identity pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Federated identities**\.

1. Choose the name of the identity pool for which you want to enable Amazon Cognito user pools as a provider\.

1. On the **Dashboard** page, choose **Edit identity pool**\.

1. Expand the **Authentication providers** section\.

1. Choose **Cognito**\.

1. Enter the **User Pool ID**\.

1. Enter the **App Client ID**\. This must be the same client app ID that you received when you created the app in the **User pools** section of the console\.

1. If you have additional apps or user pools, choose **Add Another Provider** and enter the **User Pool ID** and **App Client ID** for each app in each user pool\.

1. When you have no more apps or user pools to add, choose **Save Changes**\. If successful, you will see a **Changes saved successfully** message on the **Dashboard** page\.

------

## Integrating a user pool with an identity pool<a name="amazon-cognito-integrating-user-pools-with-identity-pools-using"></a>

After your app user is authenticated, add that user's identity token to the logins map in the credentials provider\. The provider name will depend on your Amazon Cognito user pool ID\. It will have the following structure:

```
cognito-idp.<region>.amazonaws.com/<YOUR_USER_POOL_ID>
```

The value for *<region>* will be the same as the region in the **User Pool ID**\. For example, `cognito-idp.us-east-1.amazonaws.com/us-east-1_123456789`\.

------
#### [ JavaScript ]

```
var cognitoUser = userPool.getCurrentUser();

if (cognitoUser != null) {
	cognitoUser.getSession(function(err, result) {
		if (result) {
			console.log('You are now logged in.');

			// Add the User's Id Token to the Cognito credentials login map.
			AWS.config.credentials = new AWS.CognitoIdentityCredentials({
				IdentityPoolId: 'YOUR_IDENTITY_POOL_ID',
				Logins: {
					'cognito-idp.<region>.amazonaws.com/<YOUR_USER_POOL_ID>': result.getIdToken().getJwtToken()
				}
			});
		}
	});
}
```

------
#### [ Android ]

```
cognitoUser.getSessionInBackground(new AuthenticationHandler() {
	@Override
	public void onSuccess(CognitoUserSession session) {
		String idToken = session.getIdToken().getJWTToken();

		Map<String, String> logins = new HashMap<String, String>();
		logins.put("cognito-idp.<region>.amazonaws.com/<YOUR_USER_POOL_ID>", session.getIdToken().getJWTToken());
		credentialsProvider.setLogins(logins);
	}

});
```

------
#### [ iOS \- objective\-C ]

```
AWSServiceConfiguration *serviceConfiguration = [[AWSServiceConfiguration alloc] initWithRegion:AWSRegionUSEast1 credentialsProvider:nil];
AWSCognitoIdentityUserPoolConfiguration *userPoolConfiguration = [[AWSCognitoIdentityUserPoolConfiguration alloc] initWithClientId:@"YOUR_CLIENT_ID"  clientSecret:@"YOUR_CLIENT_SECRET" poolId:@"YOUR_USER_POOL_ID"];
[AWSCognitoIdentityUserPool registerCognitoIdentityUserPoolWithConfiguration:serviceConfiguration userPoolConfiguration:userPoolConfiguration forKey:@"UserPool"];
AWSCognitoIdentityUserPool *pool = [AWSCognitoIdentityUserPool CognitoIdentityUserPoolForKey:@"UserPool"];
AWSCognitoCredentialsProvider *credentialsProvider = [[AWSCognitoCredentialsProvider alloc] initWithRegionType:AWSRegionUSEast1 identityPoolId:@"YOUR_IDENTITY_POOL_ID" identityProviderManager:pool];
```

------
#### [ iOS \- swift ]

```
let serviceConfiguration = AWSServiceConfiguration(region: .USEast1, credentialsProvider: nil)
let userPoolConfiguration = AWSCognitoIdentityUserPoolConfiguration(clientId: "YOUR_CLIENT_ID", clientSecret: "YOUR_CLIENT_SECRET", poolId: "YOUR_USER_POOL_ID")
AWSCognitoIdentityUserPool.registerCognitoIdentityUserPoolWithConfiguration(serviceConfiguration, userPoolConfiguration: userPoolConfiguration, forKey: "UserPool")
let pool = AWSCognitoIdentityUserPool(forKey: "UserPool")
let credentialsProvider = AWSCognitoCredentialsProvider(regionType: .USEast1, identityPoolId: "YOUR_IDENTITY_POOL_ID", identityProviderManager:pool)
```

------