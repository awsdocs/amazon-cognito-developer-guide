# Common Amazon Cognito Scenarios<a name="cognito-scenarios"></a>

This topic describes six common scenarios for using Amazon Cognito\.

The two main components of Amazon Cognito are user pools and identity pools\. User pools are user directories that provide sign\-up and sign\-in options for your web and mobile app users\. Identity pools provide AWS credentials to grant your users access to other AWS services\.

A user pool is a user directory in Amazon Cognito\. Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\. Whether your users sign in directly or through a third party, all members of the user pool have a directory profile that you can access through an SDK\.

With an identity pool, your users can obtain temporary AWS credentials to access AWS services, such as Amazon S3 and DynamoDB\. Identity pools support anonymous guest users, as well as federation through third\-party IdPs\.

**Topics**
+ [Authenticate with a User Pool](#scenario-basic-user-pool)
+ [Access Your Server\-side Resources with a User Pool](#scenario-backend)
+ [Access Resources with API Gateway and Lambda with a User Pool](#scenario-api-gateway)
+ [Access AWS Services with a User Pool and an Identity Pool](#scenario-aws-and-user-pool)
+ [Authenticate with a Third Party and Access AWS Services with an Identity Pool](#scenario-identity-pool)
+ [Access AWS AppSync Resources with Amazon Cognito](#scenario-appsync)

## Authenticate with a User Pool<a name="scenario-basic-user-pool"></a>

You can enable your users to authenticate with a user pool\. Your app users can sign in either directly through a user pool, or federate through a third\-party identity provider \(IdP\)\. The user pool manages the overhead of handling the tokens that are returned from social sign\-in through Facebook, Google, Amazon, and Apple, and from OpenID Connect \(OIDC\) and SAML IdPs\.

After a successful authentication, your web or mobile app will receive user pool tokens from Amazon Cognito\. You can use those tokens to retrieve AWS credentials that allow your app to access other AWS services, or you might choose to use them to control access to your server\-side resources, or to the Amazon API Gateway\.

For more information, see [User Pool Authentication Flow](amazon-cognito-user-pools-authentication-flow.md) and [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

## Access Your Server\-side Resources with a User Pool<a name="scenario-backend"></a>

After a successful user pool sign\-in, your web or mobile app will receive user pool tokens from Amazon Cognito\. You can use those tokens to control access to your server\-side resources\. You can also create user pool groups to manage permissions, and to represent different types of users\. For more information on using groups to control access your resources see [Adding Groups to a User Pool](cognito-user-pools-user-groups.md)\. 

![\[Access your server-side resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access your server-side resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access your server-side resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Once you configure a domain for your user pool, Amazon Cognito provisions a hosted web UI that allows you to add sign\-up and sign\-in pages to your app\. Using this OAuth 2\.0 foundation you can create your own resource server to enable your users to access protected resources\. For more information see [Defining Resource Servers for Your User Pool](cognito-user-pools-define-resource-servers.md)\.

For more information about user pool authentication see [User Pool Authentication Flow](amazon-cognito-user-pools-authentication-flow.md) and [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

## Access Resources with API Gateway and Lambda with a User Pool<a name="scenario-api-gateway"></a>

You can enable your users to access your API through API Gateway\. API Gateway validates the tokens from a successful user pool authentication, and uses them to grant your users access to resources including Lambda functions, or your own API\.

You can use groups in a user pool to control permissions with API Gateway by mapping group membership to IAM roles\. The groups that a user is a member of are included in the ID token provided by a user pool when your app user signs in\. For more information on user pool groups See [Adding Groups to a User Pool](cognito-user-pools-user-groups.md)\.

You can submit your user pool tokens with a request to API Gateway for verification by an Amazon Cognito authorizer Lambda function\. For more information on API Gateway, see [Using API Gateway with Amazon Cognito user pools](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)\.

![\[Access API Gateway through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access API Gateway through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access API Gateway through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

## Access AWS Services with a User Pool and an Identity Pool<a name="scenario-aws-and-user-pool"></a>

After a successful user pool authentication, your app will receive user pool tokens from Amazon Cognito\. You can exchange them for temporary access to other AWS services with an identity pool\. For more information, see [Accessing AWS Services Using an Identity Pool After Sign\-in](amazon-cognito-integrating-user-pools-with-identity-pools.md) and [Getting Started with Amazon Cognito Identity Pools \(Federated Identities\)](getting-started-with-identity-pools.md)\.

![\[Access AWS credentials through a user pool with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access AWS credentials through a user pool with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access AWS credentials through a user pool with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

## Authenticate with a Third Party and Access AWS Services with an Identity Pool<a name="scenario-identity-pool"></a>

You can enable your users access to AWS services through an identity pool\. An identity pool requires an IdP token from a user that's authenticated by a third\-party identity provider \(or nothing if it's an anonymous guest\)\. In exchange, the identity pool grants temporary AWS credentials that you can use to access other AWS services\. For more information, see [Getting Started with Amazon Cognito Identity Pools \(Federated Identities\)](getting-started-with-identity-pools.md)\.

![\[Access AWS credentials through a third-party identity provider with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access AWS credentials through a third-party identity provider with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access AWS credentials through a third-party identity provider with an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

## Access AWS AppSync Resources with Amazon Cognito<a name="scenario-appsync"></a>

You can grant your users access to AWS AppSync resources with tokens from a successful Amazon Cognito authentication \(from a user pool or an identity pool\)\. For more information, see [Access AWS AppSync and Data Sources with User Pools or Federated Identities](https://docs.aws.amazon.com/appsync/latest/devguide/security.html)\.

![\[Access AWS AppSync resources through a user pool or an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access AWS AppSync resources through a user pool or an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Access AWS AppSync resources through a user pool or an identity pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)