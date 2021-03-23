# Adding User Pool Sign\-in Through a Third Party<a name="cognito-user-pools-identity-federation"></a>

Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\. With the built\-in hosted web UI, Amazon Cognito provides token handling and management for authenticated users from all identity providers, so your backend systems can standardize on one set of user pool tokens\.

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

**Topics**
+ [Adding Social Identity Providers to a User Pool](cognito-user-pools-social-idp.md)
+ [Adding SAML Identity Providers to a User Pool](cognito-user-pools-saml-idp.md)
+ [Adding OIDC Identity Providers to a User Pool](cognito-user-pools-oidc-idp.md)
+ [Specifying Identity Provider Attribute Mappings for Your User Pool](cognito-user-pools-specifying-attribute-mapping.md)