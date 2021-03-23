# Identity Pools \(Federated Identities\) External Identity Providers<a name="external-identity-providers"></a>

Using the `logins` property, you can set credentials received from an identity provider\. Moreover, you can associate an identity pool with multiple identity providers\. For example, you could set both the Facebook and Google tokens in the `logins` property, so that the unique Amazon Cognito identity would be associated with both identity provider logins\. No matter which account the end user uses for authentication, Amazon Cognito returns the same user identifier\.

The instructions below guide you through authentication with the identity providers supported by Amazon Cognito identity pools\.

**Topics**
+ [Facebook \(Identity Pools\)](facebook.md)
+ [Login with Amazon \(Identity Pools\)](amazon.md)
+ [Google \(Identity Pools\)](google.md)
+ [Sign in with Apple \(Identity Pools\)](apple.md)
+ [Open ID Connect Providers \(Identity Pools\)](open-id.md)
+ [SAML Identity Providers \(Identity Pools\)](saml-identity-provider.md)