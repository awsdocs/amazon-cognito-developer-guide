# Caching tokens<a name="amazon-cognito-user-pools-using-tokens-caching-tokens"></a>

Your app must successfully complete one of the following requests each time you want to get a new JSON web token \(JWT\)\.
+ Request a client credentials or authorization code [grant](https://www.rfc-editor.org/rfc/rfc6749#section-1.3) from the [Token endpoint](token-endpoint.md)\.
+ Request an implicit grant from your hosted UI\.
+ Authenticate a native user in an Amazon Cognito API request like [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html)\.

You can configure your user pool to set tokens to expire in minutes, hours or days\. To ensure the performance and availability of your app, use Amazon Cognito tokens until they expire, and only then retrieve new tokens\. A cache solution that you build for your app keeps tokens available, and prevents the rejection of requests by Amazon Cognito when your request rate is too high\. A client\-side app must store tokens in a memory cache\. A server\-side app can add an encrypted cache mechanism to store tokens\.

When your user pool generates a high volume of user or machine\-to\-machine activity, you might encounter the limits that Amazon Cognito sets on the number of requests for tokens that you can make\. To reduce the number of requests you make to Amazon Cognito endpoints, you can either securely store and reuse authentication data, or implement exponential backoff and retries\.

Authentication data comes from two classes of endpoints\. Amazon Cognito [OAuth 2\.0 endpoints](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html) include the token endpoint, which services client credentials and hosted UI authorization code requests\. [Service endpoints](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html#cognito_identity_your_user_pools_region) answer native API requests like `InitiateAuth` and `RespondToAuthChallenge`\. Each type of request has its own limit\. For more information about limits, see [Quotas in Amazon Cognito](limits.md)\.

## Caching machine\-to\-machine access tokens with Amazon API Gateway<a name="amazon-cognito-user-pools-using-tokens-caching-tokens-API-gateway"></a>

With API Gateway token caching, your app can scale in response to events larger than the default request rate quota of Amazon Cognito OAuth endpoints\.

You can cache the access tokens so that your app only requests a new access token if a cached token is expired\. Otherwise, your caching endpoint returns a token from the cache\. This prevents an additional call to an Amazon Cognito API endpoint\. When you use Amazon API Gateway as a proxy to the [Token endpoint](token-endpoint.md), your API responds to the majority of requests that would otherwise contribute to your request quota, avoiding unsuccessful requests as a result of rate limiting\.

The following API Gateway\-based solution offers a low\-latency, low\-code/no\-code implementation of token caching\. API Gateway APIs are encrypted in transit, and optionally at rest\. An API Gateway cache is ideal for the OAuth 2\.0 [client credentials grant](https://datatracker.ietf.org/doc/html/rfc6749#section-4.4), a frequently high\-volume grant type that produces access tokens to authorize machine\-to\-machine and microservice sessions\. In an event like a traffic surge that causes your microservices to horizontally scale, you can end up with many systems using the same client credentials at a volume that exceeds the AWS request\-rate limit of your user pool or app client\. To preserve app availability and low latency, a caching solution is best practice in such scenarios\.

In this solution, you define a cache in your API to store a separate access token for each combination of OAuth scopes and app client that you want to request in your app\. When your app makes a request that matches the cache key, your API responds with an access token that Amazon Cognito issued to the first request that matched the cache key\. When your cache key duration expires, your API forwards the request to your token endpoint and caches a new access token\.

**Note**  
Your cache key duration must be shorter than the access token duration of your app client\.

The cache key is a combination of the OAuth scopes that you request in the `scope` URL parameter and the `Authorization` header in the request\. The `Authorization` header contains your app client ID and client secret\. You don't need to implement additional logic in your app to implement this solution\. You must only update your configuration to change the path to your user pool token endpoint\.

You can also implement token caching with [ElastiCache for Redis](https://docs.aws.amazon.com/elasticache/index.html)\. For fine\-grained control with AWS Identity and Access Management \(IAM\) policies, consider an [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/authentication-and-access-control.html#authentication) cache\.

**Note**  
Caching in API Gateway is subject to additional cost\. [See pricing for more details\.](https://aws.amazon.com/api-gateway/pricing)<a name="amazon-cognito-user-pools-using-tokens-caching-tokens-API-gateway-how-to"></a>

## To set up a caching proxy with API Gateway<a name="amazon-cognito-user-pools-using-tokens-caching-tokens-API-gateway-how-to"></a>

1. Open the [API Gateway console](https://console.aws.amazon.com/apigateway/main/apis) and create a REST API\.

1. In **Resources**, create a POST method\.

   1. Choose the HTTP **Integration type**\.

   1. Select **Use HTTP proxy integration**\.

   1. Enter an **Endpoint URL** of `https://<your user pool domain>/oauth2/token`\.

1. In **Resources**, configure the cache key\.

   1. Edit the **Method request** of your POST method\.

   1. Set your `scope` parameter and `Authorization` header as your caching key\.

      1. Add a query string to **URL query string parameters** and choose **Caching** for the `scope` string\.

      1. Add a header to **HTTP request headers** and choose **Caching** for the `Authorization` header\.

1. In **Stages**, configure caching\.

   1. Choose the stage that you want to modify\.

   1. Under **Settings**, select **Enable API cache**\.

   1. Choose a **Cache capacity**\.

   1. Choose a **Cache time\-to\-live \(TTL\)** of at least 3600 seconds\.

   1. Clear the **Require authorization** checkbox\.

1. In **Stages**, note the **Invoke URL**\.

1. Update your app to POST token requests to the **Invoke URL** of your API instead of the `/oauth2/token` endpoint of your user pool\.