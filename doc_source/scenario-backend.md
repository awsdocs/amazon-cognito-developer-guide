# Accessing server\-side resources after sign\-in<a name="scenario-backend"></a>

After a successful authentication, your web or mobile app will receive user pool tokens from Amazon Cognito\. You can use those tokens to control access to your server\-side resources\. You can also create user pool groups to manage permissions, and to represent different types of users\. For more information on using groups to control access your resources see [Adding groups to a user pool](cognito-user-pools-user-groups.md)\. 

![\[Accessing your backend resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing your backend resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing your backend resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Once you configure a domain for your user pool, Amazon Cognito provisions a hosted web UI that allows you to add sign\-up and sign\-in pages to your app\. Using this OAuth 2\.0 foundation you can create your own resource server to enable your users to access protected resources\. For more information see [Defining resource servers for your user pool](cognito-user-pools-define-resource-servers.md)\.

For more information about user pool authentication see [User pool authentication flow](amazon-cognito-user-pools-authentication-flow.md) and [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.