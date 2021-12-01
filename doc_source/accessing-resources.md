# Accessing resources after a successful user pool authentication<a name="accessing-resources"></a>

Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\. For more information see [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\. 

After a successful authentication, your app will receive user pool tokens from Amazon Cognito\. You can use those tokens to retrieve AWS credentials that allow your app to access other AWS services, or you might choose to use them to control access to your own server\-side resources, or to the Amazon API Gateway\.

For more information, see [User pool authentication flow](amazon-cognito-user-pools-authentication-flow.md) and [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Topics**
+ [Accessing server\-side resources after sign\-in](scenario-backend.md)
+ [Accessing resources with API Gateway and Lambda after sign\-in](user-pool-accessing-resources-api-gateway-and-lambda.md)
+ [Accessing AWS services using an identity pool after sign\-in](amazon-cognito-integrating-user-pools-with-identity-pools.md)