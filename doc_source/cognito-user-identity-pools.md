# Amazon Cognito user pools<a name="cognito-user-identity-pools"></a>

A user pool is a user directory in Amazon Cognito\. With a user pool, your users can sign in to your web or mobile app through Amazon Cognito\. Your users can also sign in through social identity providers like Google, Facebook, Amazon, or Apple, and through SAML identity providers\. Whether your users sign in directly or through a third party, all members of the user pool have a directory profile that you can access through a Software Development Kit \(SDK\)\.

User pools provide:
+ Sign\-up and sign\-in services\.
+ A built\-in, customizable web UI to sign in users\.
+ Social sign\-in with Facebook, Google, Login with Amazon, and Sign in with Apple, as well as sign\-in with SAML identity providers from your user pool\.
+ User directory management and user profiles\.
+ Security features such as multi\-factor authentication \(MFA\), checks for compromised credentials, account takeover protection, and phone and email verification\.
+ Customized workflows and user migration through AWS Lambda triggers\.

After successfully authenticating a user, Amazon Cognito issues JSON web tokens \(JWT\) that you can use to secure and authorize access to your own APIs, or exchange for AWS credentials\.

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Amazon Cognito provides token handling through the Amazon Cognito user pools Identity SDKs for JavaScript, Android, and iOS\. See [Getting started with user pools](getting-started-with-cognito-user-pools.md) and [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

The two main components of Amazon Cognito are user pools and identity pools\. Identity pools provide AWS credentials to grant your users access to other AWS services\. To enable users in your user pool to access AWS resources, you can configure an identity pool to exchange user pool tokens for AWS credentials\. For more information see [Accessing AWS services using an identity pool after sign\-in](amazon-cognito-integrating-user-pools-with-identity-pools.md) and [Getting started with Amazon Cognito identity pools \(federated identities\)](getting-started-with-identity-pools.md)\.

**Topics**
+ [Getting started with user pools](getting-started-with-cognito-user-pools.md)
+ [Using the Amazon Cognito hosted UI for sign\-up and sign\-in](cognito-user-pools-app-integration.md)
+ [Adding user pool sign\-in through a third party](cognito-user-pools-identity-federation.md)
+ [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)
+ [Using Amazon Pinpoint analytics with Amazon Cognito user pools](cognito-user-pools-pinpoint-integration.md)
+ [Managing users in your user pool](managing-users.md)
+ [Email settings for Amazon Cognito user pools](user-pool-email.md)
+ [SMS message settings for Amazon Cognito user pools](user-pool-sms-settings.md)
+ [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)
+ [Accessing resources after a successful user pool authentication](accessing-resources.md)
+ [User pools reference \(AWS Management Console\)](cognito-user-pools-getting-started-step-through-settings.md)
+ [Managing error responses](cognito-user-pool-managing-errors.md)