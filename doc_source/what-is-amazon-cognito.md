# What is Amazon Cognito?<a name="what-is-amazon-cognito"></a>

Amazon Cognito provides authentication, authorization, and user management for your web and mobile apps\. Your users can sign in directly with a user name and password, or through a third party such as Facebook, Amazon, Google or Apple\.

The two main components of Amazon Cognito are user pools and identity pools\. User pools are user directories that provide sign\-up and sign\-in options for your app users\. Identity pools enable you to grant your users access to other AWS services\. You can use identity pools and user pools separately or together\.

**An Amazon Cognito user pool and identity pool used together**

See the diagram for a common Amazon Cognito scenario\. Here the goal is to authenticate your user, and then grant your user access to another AWS service\.

1. In the first step, your app user signs in through a user pool and receives user pool tokens after a successful authentication\.

1. Next, your app exchanges the user pool tokens for AWS credentials through an identity pool\.

1. Finally, your app user can then use those AWS credentials to access other AWS services such as Amazon S3 or DynamoDB\.

![\[Amazon Cognito overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Amazon Cognito overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Amazon Cognito overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

For more examples using identity pools and user pools, see [Common Amazon Cognito scenarios](cognito-scenarios.md)\.

Amazon Cognito is compliant with SOC 1\-3, PCI DSS, ISO 27001, and is HIPAA\-BAA eligible\. For more information, see [AWS services in scope](http://aws.amazon.com/compliance/services-in-scope/)\. See also [Regional data considerations](security-cognito-regional-data-considerations.md)\.

**Topics**
+ [Features of Amazon Cognito](#feature-overview)
+ [Getting started with Amazon Cognito](#getting-started-overview)
+ [Regional availability](#getting-started-regional-availability)
+ [Pricing for Amazon Cognito](#pricing-for-amazon-cognito)
+ [Using the Amazon Cognito console](cognito-console.md)

## Features of Amazon Cognito<a name="feature-overview"></a>

**User pools**

A user pool is a user directory in Amazon Cognito\. With a user pool, your users can sign in to your web or mobile app through Amazon Cognito, or federate through a third\-party identity provider \(IdP\)\. Whether your users sign in directly or through a third party, all members of the user pool have a directory profile that you can access through an SDK\.

User pools provide:
+ Sign\-up and sign\-in services\.
+ A built\-in, customizable web UI to sign in users\.
+ Social sign\-in with Facebook, Google, Login with Amazon, and Sign in with Apple, and through SAML and OIDC identity providers from your user pool\.
+ User directory management and user profiles\.
+ Security features such as multi\-factor authentication \(MFA\), checks for compromised credentials, account takeover protection, and phone and email verification\.
+ Customized workflows and user migration through AWS Lambda triggers\.

For more information about user pools, see [Getting started with user pools](getting-started-with-cognito-user-pools.md) and the [Amazon Cognito user pools API reference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/)\.

**Identity pools**

With an identity pool, your users can obtain temporary AWS credentials to access AWS services, such as Amazon S3 and DynamoDB\. Identity pools support anonymous guest users, as well as the following identity providers that you can use to authenticate users for identity pools:
+ Amazon Cognito user pools
+ Social sign\-in with Facebook, Google, Login with Amazon, and Sign in with Apple
+ OpenID Connect \(OIDC\) providers
+ SAML identity providers
+ Developer authenticated identities

To save user profile information, your identity pool needs to be integrated with a user pool\.

For more information about identity pools, see [Getting started with Amazon Cognito identity pools \(federated identities\)](getting-started-with-identity-pools.md) and the [Amazon Cognito identity pools API reference](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/)\.

## Getting started with Amazon Cognito<a name="getting-started-overview"></a>

For a guide to top tasks and where to start, see [Getting started with Amazon Cognito](cognito-getting-started.md)\.

For videos, articles, documentation, and sample apps, see [Amazon Cognito developer resources](https://aws.amazon.com/cognito/dev-resources/)\.

To use Amazon Cognito, you need an AWS account\. For more information, see [Using the Amazon Cognito console](cognito-console.md)\.

## Regional availability<a name="getting-started-regional-availability"></a>

Amazon Cognito is available in multiple AWS Regions worldwide\. In each Region, Amazon Cognito is distributed across multiple Availability Zones\. These Availability Zones are physically isolated from each other, but are united by private, low\-latency, high\-throughput, and highly redundant network connections\. These Availability Zones enable AWS to provide services, including Amazon Cognito, with very high levels of availability and redundancy, while also minimizing latency\.

For a list of all the Regions where Amazon Cognito is currently available, see [AWS regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html##cognito_identity_region) in the *Amazon Web Services General Reference*\. To learn more about the number of Availability Zones that are available in each Region, see [AWS global infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

## Pricing for Amazon Cognito<a name="pricing-for-amazon-cognito"></a>

For information about Amazon Cognito pricing, see [Amazon Cognito pricing](https://aws.amazon.com/cognito/pricing/)\.
