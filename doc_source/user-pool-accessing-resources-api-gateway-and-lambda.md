# Accessing resources with API Gateway and Lambda after sign\-in<a name="user-pool-accessing-resources-api-gateway-and-lambda"></a>

You can enable your users to access your API through API Gateway\. API Gateway validates the tokens from a successful user pool authentication, and uses them to grant your users access to resources including Lambda functions, or your own API\.

![\[Accessing API Gateway\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing API Gateway\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Accessing API Gateway\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

You can use groups in a user pool to control permissions with API Gateway by mapping group membership to IAM roles\. The groups that a user is a member of are included in the ID token provided by a user pool when your web or mobile app user signs in\. For more information on user pool groups See [Adding groups to a user pool](cognito-user-pools-user-groups.md)\.

You can submit your user pool tokens with a request to API Gateway for verification by an Amazon Cognito authorizer Lambda function\. For more information on API Gateway, see [Using API Gateway with Amazon Cognito user pools](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html)\.