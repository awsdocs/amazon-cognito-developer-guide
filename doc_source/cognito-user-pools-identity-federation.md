# Adding user pool sign\-in through a third party<a name="cognito-user-pools-identity-federation"></a>

Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\. With the built\-in hosted web UI, Amazon Cognito provides token handling and management for authenticated users from all identity providers\. This way, your backend systems can standardize on one set of user pool tokens\.

Amazon Cognito includes the `user-agent` header `Amazon/Cognito` in hosted UI redirect requests to federated identity providers\.

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

**Topics**
+ [Adding social identity providers to a user pool](cognito-user-pools-social-idp.md)
+ [Adding SAML identity providers to a user pool](cognito-user-pools-saml-idp.md)
+ [Adding OIDC identity providers to a user pool](cognito-user-pools-oidc-idp.md)
+ [Specifying identity provider attribute mappings for your user pool](cognito-user-pools-specifying-attribute-mapping.md)