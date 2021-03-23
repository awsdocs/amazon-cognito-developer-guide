# Defining Resource Servers for Your User Pool<a name="cognito-user-pools-define-resource-servers"></a>

Once you configure a domain for your user pool, the Amazon Cognito service automatically provisions a hosted web UI that allows you to add sign\-up and sign\-in pages to your app\. For more information see [Step 2\. Add an App to Enable the Hosted Web UI](cognito-user-pools-configuring-app-integration.md)\.

A *resource server* is a server for access\-protected resources\. It handles authenticated requests from an app that has an access token\. Typically the resource server provides a [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) API for making these access requests\. This API can be hosted in Amazon API Gateway or outside of AWS\. The app passes the access token in the API call to the resource server\. The app should treat the access token as opaque when it passes the token in the access request\. The resource server inspects the access token to determine if access should be granted\. 

**Note**  
Your resource server must verify the access token signature and expiration date before processing any claims inside the token\. For more information about verifying and using user pool tokens, see [this blog post](https://aws.amazon.com/blogs/mobile/integrating-amazon-cognito-user-pools-with-api-gateway/)\. Amazon API Gateway is a good option for inspecting access tokens and protecting your resources\. For more about API Gateway custom authorizers, see [Use API Gateway Custom Authorizers](https://docs.aws.amazon.com/apigateway/latest/developerguide/use-custom-authorizer.html)\.

A *scope* is a level of access that an app can request to a resource\. For example, if you have a resource server for photos, it might define two scopes: one for read access to the photos and one for write/delete access\. When the app makes an API call to request access and passes an access token, the token will have one or more scopes embedded in inside it\.

**Overview**

Amazon Cognito allows app developers to create their own OAuth2\.0 resource servers and define custom scopes in them\. Custom scopes can then be associated with a client, and the client can request them in OAuth2\.0 authorization code grant flow, implicit flow, and client credentials flow\. Custom scopes are added in the `scope` claim in the access token\. A client can use the access token against its resource server, which makes the authorization decision based on scopes present in the token\. For more information about access token scope, see [Using Tokens with User Pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.

**Note**  
Your resource server must verify the access token signature and expiration date before processing any claims inside the token\.

**Note**  
An app client can only use the client credentials flow if the app client has a client secret\.

**Managing the Resource Server and Custom Scopes**

When creating a resource server, you must provide a resource server name and a resource server identifier\. For each scope you create in the resource server, you must provide the scope name and description\.

Example:
+ `Name`: a friendly name for the resource server, such as `Weather API` or `Photo API`\.
+ `Identifier`: Unique identifier for the resource server\. This could be an HTTPS endpoint where your resource server is located\. For example, `https://my-weather-api.example.com`
+ `Scope Name`: The scope name\. For example, `weather.read`
+ `Scope Description`: A brief description the scope\. For example, `Retrieve weather information`\.

When a client app requests a custom scope in any of the OAuth2\.0 flows, it must request the full identifier for the scope, which is `resourceServerIdentifier/scopeName`\. For example, if the resource server identifier is `https://myphotosapi.example.com` and the scope name is `photos.read`, the client app must request `https://myphotosapi.example.com/photos.read` at runtime\.

Deleting a scope from a resource server does not delete its association with all clients; deleting the scope makes it inactive\. So if a client app requests the deleted scope at runtime, the scope is ignored and is not included in the access token\. If the scope is re\-added later, then it is again included in the access token\.

If a scope is removed from a client, the association between client and scope is deleted\. If a client requests a disallowed scope at runtime, this results in an error, and an access token is not issued\.

You can use the AWS Management Console, API, and CLI to define resource servers and scopes for your user pool\.

## Defining a Resource Server for Your User Pool \(AWS Management Console\)<a name="cognito-user-pools-define-resource-servers-console"></a>

You can use the AWS Management Console to define a resource server for your user pool\.

**To define a resource server**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, **Manage User Pools**, and choose the user pool you want to edit\.

1. Choose the **Resource servers** tab\.

1. Choose **Add a resource server**\.

1. Enter the name of your resource server, for example, `Photo Server`\.

1. Enter the identifier of your resource server, for example, `com.example.photos`\.

1. Enter the names of the custom scopes for your resources, such as `read` and `write`\.

1. For each of the scope names, enter a description, such as `view your photos` and `update your photos`\.

1. Choose **Save changes**\.

Each of the custom scopes that you define appears on the **App client settings** tab, under **OAuth2\.0 Allowed Custom Scopes**; for example `com.example.photos/read`\.

## Defining a Resource Server for Your User Pool \(AWS CLI and AWS API\)<a name="cognito-user-pools-define-resource-servers-cli-api"></a>

Use the following commands to specify resource server settings for your user pool\.

**To create a resource server**
+ AWS CLI: `aws cognito-idp create-resource-server`
+ AWS API: [CreateResourceServer](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateResourceServer.html)

**To get information about your resource server settings**
+ AWS CLI: `aws cognito-idp describe-resource-server`
+ AWS API: [DescribeResourceServer](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DescribeResourceServer.html)

**To list information about all resource servers for your user pool**
+ AWS CLI: `aws cognito-idp list-resource-servers`
+ AWS API: [ListResourceServers](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListResourceServers.html)

**To delete a resource server**
+ AWS CLI: `aws cognito-idp delete-resource-server`
+ AWS API: [DeleteResourceServer](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DeleteResourceServer.html)

**To update the settings for a resource server**
+ AWS CLI: `aws cognito-idp update-resource-server`
+ AWS API: [UpdateResourceServer](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateResourceServer.html)