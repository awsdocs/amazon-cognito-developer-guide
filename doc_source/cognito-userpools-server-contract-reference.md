# Amazon Cognito user pools Auth API Reference<a name="cognito-userpools-server-contract-reference"></a>

Once a domain has been configured for your user pool, Amazon Cognito hosts an authentication server that allows you to add sign\-up and sign\-in webpages to your app\. For more information see [Add an app to enable the hosted UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-app-integration.html)\.

This section contains the HTTPS contract to the Amazon Cognito authentication server from a user pool client, including sample requests and responses\. It describes the expected behavior from the authentication server for positive and negative conditions\.

In addition to the server contract REST API, Amazon Cognito also provides Auth SDKs for Android, iOS, and JavaScript that make it easier to form requests and interact with the server\. Learn more about the [Amazon Cognito SDKs](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

For more information on the specification see [OpenID Connect 1\.0](http://openid.net/specs/openid-connect-core-1_0.html) and [OAuth 2\.0](https://tools.ietf.org/html/rfc6749)\.

**Topics**
+ [AUTHORIZATION Endpoint](authorization-endpoint.md)
+ [TOKEN Endpoint](token-endpoint.md)
+ [USERINFO Endpoint](userinfo-endpoint.md)
+ [LOGIN Endpoint](login-endpoint.md)
+ [LOGOUT Endpoint](logout-endpoint.md)