# Accessing Server\-side Resources after Sign\-in<a name="scenario-backend"></a>

After a successful authentication, your web or mobile app will receive user pool tokens from Amazon Cognito\. You can use those tokens to control access to your server\-side resources\. You can also create user pool groups to manage permissions, and to represent different types of users\. For more information on using groups to control access your resources see [Adding Groups to a User Pool](cognito-user-pools-user-groups.md)\. 

![\[Accessing your backend resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing your backend resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing your backend resources through a user pool\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Once you configure a domain for your user pool, Amazon Cognito provisions a hosted web UI that allows you to add sign\-up and sign\-in pages to your app\. Using this OAuth 2\.0 foundation you can create your own resource server to enable your users to access protected resources\. For more information see [Defining Resource Servers for Your User Pool](cognito-user-pools-define-resource-servers.md)\.

For more information about user pool authentication see [User Pool Authentication Flow](amazon-cognito-user-pools-authentication-flow.md) and [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.