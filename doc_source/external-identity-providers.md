# Identity pools \(federated identities\) external identity providers<a name="external-identity-providers"></a>

Using the `logins` property, you can set credentials received from an identity provider \(IdP\)\. Furthermore, you can associate an identity pool with multiple IdPs\. For example, you can set both the Facebook and Google tokens in the `logins` property to associate the unique Amazon Cognito identity with both IdP logins\. The user can authenticate with either account, but Amazon Cognito returns the same user identifier\.

The instructions below guide you through authentication with the IdPs that Amazon Cognito identity pools support\.

**Topics**
+ [Facebook \(identity pools\)](facebook.md)
+ [Login with Amazon \(identity pools\)](amazon.md)
+ [Google \(identity pools\)](google.md)
+ [Sign in with Apple \(identity pools\)](apple.md)
+ [Open ID Connect providers \(identity pools\)](open-id.md)
+ [SAML identity providers \(identity pools\)](saml-identity-provider.md)