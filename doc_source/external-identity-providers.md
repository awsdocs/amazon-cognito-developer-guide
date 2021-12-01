# Identity pools \(federated identities\) external identity providers<a name="external-identity-providers"></a>

Using the `logins` property, you can set credentials received from an identity provider\. Moreover, you can associate an identity pool with multiple identity providers\. For example, you could set both the Facebook and Google tokens in the `logins` property, so that the unique Amazon Cognito identity would be associated with both identity provider logins\. No matter which account the end user uses for authentication, Amazon Cognito returns the same user identifier\.

The instructions below guide you through authentication with the identity providers supported by Amazon Cognito identity pools\.

**Topics**
+ [Facebook \(identity pools\)](facebook.md)
+ [Login with Amazon \(identity pools\)](amazon.md)
+ [Google \(identity pools\)](google.md)
+ [Sign in with Apple \(identity pools\)](apple.md)
+ [Open ID Connect providers \(identity pools\)](open-id.md)
+ [SAML identity providers \(identity pools\)](saml-identity-provider.md)