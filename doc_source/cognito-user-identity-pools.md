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

Amazon Cognito provides token handling through the Amazon Cognito user pools Identity SDKs for JavaScript, Android, and iOS\. See [Getting Started with User Pools](getting-started-with-cognito-user-pools.md) and [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

The two main components of Amazon Cognito are user pools and identity pools\. Identity pools provide AWS credentials to grant your users access to other AWS services\. To enable users in your user pool to access AWS resources, you can configure an identity pool to exchange user pool tokens for AWS credentials\. For more information see [Accessing AWS Services Using an Identity Pool After Sign\-in](amazon-cognito-integrating-user-pools-with-identity-pools.md) and [Getting Started with Amazon Cognito Identity Pools \(Federated Identities\)](getting-started-with-identity-pools.md)\.

**Topics**
+ [Getting Started with User Pools](getting-started-with-cognito-user-pools.md)
+ [Using the Amazon Cognito Hosted UI for Sign\-Up and Sign\-In](cognito-user-pools-app-integration.md)
+ [Adding User Pool Sign\-in Through a Third Party](cognito-user-pools-identity-federation.md)
+ [Customizing User Pool Workflows with Lambda Triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)
+ [Using Amazon Pinpoint Analytics with Amazon Cognito User Pools](cognito-user-pools-pinpoint-integration.md)
+ [Managing Users in User Pools](managing-users.md)
+ [Email Settings for Amazon Cognito User Pools](user-pool-email.md)
+ [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)
+ [Accessing Resources After a Successful User Pool Authentication](accessing-resources.md)
+ [User Pools Reference \(AWS Management Console\)](cognito-user-pools-getting-started-step-through-settings.md)
+ [Managing error responses](cognito-user-pool-managing-errors.md)