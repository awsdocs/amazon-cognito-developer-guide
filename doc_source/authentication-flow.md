# Identity pools \(federated identities\) authentication flow<a name="authentication-flow"></a>

Amazon Cognito helps you create unique identifiers for your end users that are kept consistent across devices and platforms\. Amazon Cognito also delivers temporary, limited\-privilege credentials to your application to access AWS resources\. This page covers the basics of how authentication in Amazon Cognito works and explains the life cycle of an identity inside your identity pool\.

**External provider authflow**

A user authenticating with Amazon Cognito will go through a multi\-step process to bootstrap their credentials\. Amazon Cognito has two different flows for authentication with public providers: enhanced and basic\.

Once you complete one of these flows, you can access other AWS services as defined by your role's access policies\. By default, the [Amazon Cognito console](https://console.aws.amazon.com/cognito/) will create roles with access to the Amazon Cognito Sync store and to Amazon Mobile Analytics\. For more information on how to grant additional access see [IAM roles](iam-roles.md)\.

**Enhanced \(simplified\) authflow**

1. `GetId`

1. `GetCredentialsForIdentity`

![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Basic \(classic\) authflow**

1. `GetId`

1. `GetOpenIdToken`

1. `AssumeRoleWithWebIdentity`

![\[External Provider Basic Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Basic Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[External Provider Basic Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Developer authenticated identities authflow**

When using [Developer authenticated identities \(identity pools\)](developer-authenticated-identities.md), the client will use a different authflow that will include code outside of Amazon Cognito to validate the user in your own authentication system\. Code outside of Amazon Cognito is indicated as such\.

**Enhanced authflow**

1. Login via Developer Provider \(code outside of Amazon Cognito\)

1. Validate the user's login \(code outside of Amazon Cognito\)

1. [GetOpenIdTokenForDeveloperIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetOpenIdTokenForDeveloperIdentity.html)

1. [GetCredentialsForIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetCredentialsForIdentity.html)

![\[Developer Authenticated Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Developer Authenticated Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Developer Authenticated Enhanced Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Basic authflow**

1. Login via Developer Provider \(code outside of Amazon Cognito\)

1. Validate the user's login \(code outside of Amazon Cognito\)

1. [GetOpenIdTokenForDeveloperIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetOpenIdTokenForDeveloperIdentity.html)

1. [AssumeRoleWithWebIdentity](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html)

![\[Developer Authenticated Basic Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Developer Authenticated Basic Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Developer Authenticated Basic Authflow\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Which authflow should I use?**

For most customers, the Enhanced Flow is the correct choice, as it offers many benefits over the Basic Flow:
+ One fewer network call to get credentials on the device\.
+ All calls are made to Amazon Cognito, meaning it is also one less network connection\.
+ Roles no longer need to be embedded in your application, only an identity pool id and region are necessary to start bootstrapping credentials\.

Since February 2015, the [Amazon Cognito console](https://console.aws.amazon.com/cognito/) displayed example code that used the Enhanced Flow\. Additionally, the console will display a notification if your identity pool does not have the role association necessary to use the Enhanced Flow\.

The following are the minimum SDK versions where the Enhanced Flow is supported:

**SDK \(minimum version\)**
+ AWS SDK for iOS \(2\.0\.14\)
+ AWS SDK for Android \(2\.1\.8\)
+ AWS SDK for JavaScript \(2\.1\.7\)
+ AWS SDK for Unity \(1\.0\.3\)
+ AWS SDK for Xamarin \(3\.0\.0\.5\)

You may still wish to use the Basic Flow if you want to use more than the two default roles configured when you create a new identity pool in the console\.

**API summary**

**GetId**

The GetId API call is the first call necessary to establish a new identity in Amazon Cognito\.

**Unauthenticated access**

Amazon Cognito has the ability to allow unauthenticated guest access in your applications\. If this feature is enabled in your identity pool, users can request a new identity ID at any time via the GetId API\. The application is expected to cache this identity ID to make subsequent calls to Amazon Cognito\. The AWS Mobile SDKs as well as the AWS SDK for JavaScript in the Browser have credentials providers that handle this caching for you\.

**Authenticated access**

When you've configured your application with support for a public login provider \(Facebook, Google\+, Login with Amazon, or Sign in with Apple\), users will also be able to supply tokens \(OAuth or OpenID Connect\) that identify them in those providers\. When used in a call to GetId, Amazon Cognito will either create a new authenticated identity or return the identity already associated with that particular login\. Amazon Cognito does this by validating the token with the provider and ensuring that:
+ The token is valid and from the configured provider
+ The token is not expired
+ The token matches the application identifier created with that provider \(e\.g\., Facebook app ID\)
+ The token matches the user identifier

**GetCredentialsForIdentity**

The GetCredentialsForIdentity API can be called after you establish an identity ID\. This API is functionally equivalent to calling GetOpenIdToken followed by AssumeRoleWithWebIdentity\.

In order for Amazon Cognito to call AssumeRoleWithWebIdentity on your behalf, your identity pool must have IAM roles associated with it\. You can do this via the Amazon Cognito Console or manually via the SetIdentityPoolRoles operation \(see the API reference\)

**GetOpenIdToken**

The GetOpenIdToken API call is called after you establish an identity ID\. If you have a cached identity ID, this can be the first call you make during an app session\.

**Unauthenticated access**

To obtain a token for an unauthenticated identity, you only need the identity ID itself\. It is not possible to get an unauthenticated token for authenticated or disabled identities\.

**Authenticated access**

If you have an authenticated identity, you must pass at least one valid token for a login already associated with that identity\. All tokens passed in during the GetOpenIdToken call must pass the same validation mentioned earlier; if any of the tokens fail, the whole call fails\. The response from the GetOpenIdToken call also includes the identity ID\. This is because the identity ID you pass in may not be the one that is returned\.

**Linking logins**

If you pass in a token for a login that is not already associated with any identity, the login is considered to be "linked" to the associated identity\. You may only link one login per public provider\. Attempts to link more than one login with a public provider will result in a ResourceConflictException\. If a login is merely linked to an existing identity, the identity ID returned from GetOpenIdToken will be the same as what was passed in\.

**Merging identities**

If you pass in a token for a login that is not currently linked to the given identity, but is linked to another identity, the two identities are merged\. Once merged, one identity becomes the parent/owner of all associated logins and the other is disabled\. In this case, the identity ID of the parent/owner is returned\. You are expected to update your local cache if this value differs \(this is handled for you if you are using the providers in the AWS Mobile SDKs or AWS SDK for JavaScript in the Browser\)\.

**GetOpenIdTokenForDeveloperIdentity**

The GetOpenIdTokenForDeveloperIdentity API replaces the use of GetId and GetOpenIdToken from the device when using developer authenticated identities\. Because this API call is signed by your AWS credentials, Amazon Cognito can trust that the user identifier supplied in the API call is valid\. This replaces the token validation Amazon Cognito performs with external providers\.

The payload for this API includes a logins map which must contain the key of your developer provider and a value as an identifier for the user in your system\. If the user identifier isn't already linked to an existing identity, Amazon Cognito will create a new identity and return the new identity id and an OpenId Connect token for that identity\. If the user identifier is already linked, Amazon Cognito will return the pre\-existing identity id and an OpenId Connect token\.

**Linking logins**

As with external providers, supplying additional logins that are not already associated with an identity will implicitly link those logins to that identity\. It is important to note that if you link an external provider login to an identity, the user can use the external provider authflow with that provider, but they cannot use your developer provider name in the logins map when calling GetId or GetOpenIdToken\.

**Merging identities** 

With developer authenticated identities, Amazon Cognito supports both implicit merging as well as explicit merging via the MergeDeveloperIdentities API\. This explicit merging allows you to mark two identities with user identifiers in your system as a single identity\. You simply supply the source and destination user identifiers and Amazon Cognito will merge them\. The next time you request an OpenId Connect token for either user identifier, the same identity id will be returned\.

**AssumeRoleWithWebIdentity**

Once you have an OpenID Connect token, you can then trade this for temporary AWS credentials via the AssumeRoleWithWebIdentity API call in AWS Security Token Service \(STS\)\. This call is no different than if you were using Facebook, Google\+, Login with Amazon, or Sign in with Apple directly, except that you are passing an Amazon Cognito token instead of a token from one of the other public providers\.

Because there's no restriction on the number of identities that can be created, it's important to understand the permissions that are being granted to your users\. We recommend having two different roles for your application: one for unauthenticated users, and one for authenticated users\. The Amazon Cognito console will create these for you by default when you first set up your identity pool\. The access policy for these two roles will be exactly the same: it will grant users access to Amazon Cognito Sync as well as to submit events to Amazon Mobile Analytics\. You are welcome and encouraged to modify these roles to meet your needs\.

Learn more about [Role trust and permissions](role-trust-and-permissions.md)\.