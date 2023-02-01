# Token endpoint<a name="token-endpoint"></a>

Generate a POST request to the `/oauth2/token` endpoint to get JSON web tokens \(JWTs\) for a user or service\. When you add a domain to your user pool, Amazon Cognito activates an OAuth 2\.0 [token endpoint](http://amazonaws.com/https://www.rfc-editor.org/rfc/rfc6749#section-3.2) that's dedicated to your user pool\. In a user\-based model, your app sends authorization codes to your token endpoint in exchange for ID, access, and refresh tokens\. In a machine\-to\-machine model, you app sends a client secret to your token endpoint in exchange for access tokens\.

To retrieve information about a user from their access token, you can pass it to your [UserInfo endpoint](userinfo-endpoint.md), or to a [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html) API request\.

## POST /oauth2/token<a name="post-token"></a>

The `/oauth2/token` endpoint only supports `HTTPS POST`\. Your app makes requests to this endpoint directly, not through the user's browser\.

The token endpoint supports `client_secret_basic` and `client_secret_post` authentication\. For more information from the OpenID Connect specification, see [Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication)\. For more information about the token endpoint from the OpenID Connect specification, see [Token Endpoint](http://openid.net/specs/openid-connect-core-1_0.html#TokenEndpoint)\.

### Request parameters in header<a name="post-token-request-parameters"></a>

*Authorization*  
If the client was issued a secret, the client can pass its `client_id` and `client_secret` in the authorization header as `client_secret_basic` HTTP authorization\. You can also include the `client_id` and `client_secret` in the request body as `client_secret_post` authorization\.  
The authorization header string is [Basic](https://en.wikipedia.org/wiki/Basic_access_authentication#Client_side) `Base64Encode(client_id:client_secret)`\. The following example is an authorization header for app client `djc98u3jiedmi283eu928` with client secret `abcdef01234567890`, using the Base64\-encoded version of the string `djc98u3jiedmi283eu928:abcdef01234567890`\.  

```
Authorization: Basic ZGpjOTh1M2ppZWRtaTI4M2V1OTI4OmFiY2RlZjAxMjM0NTY3ODkw
```

*Content\-Type*  
Must always be `'application/x-www-form-urlencoded'`\.

### Request parameters in body<a name="post-token-request-parameters-in-body"></a>

*grant\_type*  
Grant type\.  
Must be `authorization_code` or `refresh_token` or `client_credentials`\. You can request an access token for a custom scope from the token endpoint when, in the app client, the requested scope is enabled, you have configured a client secret, and you have allowed `client_credentials` grants\.  
Required\.

*client\_id*  
The ID of an app client in your user pool\. You must specify the same app client that authenticated your user\.  
Required if the client is public and does not have a secret, or with `client_secret` if you want to use `client_secret_post` authorization\.

*client\_secret*  
The client secret for the app client that authenticated your user\. Required if your app client has a client secret and you did not send an `Authorization` header\.

*scope*  
Can be a combination of any custom scopes associated with an app client\. Any scope that you request must be activated for the app client, or Amazon Cognito will ignore it\. If the client doesn't request any scopes, the authentication server uses all custom scopes associated with the client\.  
Optional\. Only used if the `grant_type` is `client_credentials`\.

*redirect\_uri*  
Must be the same `redirect_uri` that was used to get `authorization_code` in `/oauth2/authorize`\.  
Required only if `grant_type` is `authorization_code`\.

*refresh\_token*  
To generate new access and ID tokens for a user's session, set the value of a `refresh_token` parameter in your `/oauth2/token` request to a previously\-issued refresh token from the same app client\.

*code*  
Required if `grant_type` is `authorization_code`\.

*code\_verifier*  
The proof key\.  
Required if `grant_type` is `authorization_code` and the authorization code was requested with PKCE\.

### Examples requests with positive responses<a name="post-token-positive"></a>

#### Exchanging an authorization code for tokens<a name="post-token-positive-exchanging-authorization-code-for-tokens"></a>

 **Sample request**

```
POST https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/token&
                            Content-Type='application/x-www-form-urlencoded'&
                            Authorization=Basic ZGpjOTh1M2ppZWRtaTI4M2V1OTI4OmFiY2RlZjAxMjM0NTY3ODkw
                            
                            grant_type=authorization_code&
                            client_id=1example23456789&
                            code=AUTHORIZATION_CODE&
                            redirect_uri=com.myclientapp://myclient/redirect
```

**Sample response**

```
HTTP/1.1 200 OK
                        
                           Content-Type: application/json
                           
                           { 
                            "access_token":"eyJra1example", 
                            "id_token":"eyJra2example",
                            "refresh_token":"eyJj3example",
                            "token_type":"Bearer", 
                            "expires_in":3600
                           }
```

**Note**  
The token endpoint returns `refresh_token` only when the `grant_type` is `authorization_code`\.

#### Exchanging client credentials for an access token<a name="post-token-positive-exchanging-client-credentials-for-an-access-token"></a>

 **Sample request**

```
POST https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/token >
                            Content-Type='application/x-www-form-urlencoded'&
                            Authorization=Basic ZGpjOTh1M2ppZWRtaTI4M2V1OTI4OmFiY2RlZjAxMjM0NTY3ODkw
                            
                            grant_type=client_credentials&
                            client_id=1example23456789&
                            scope={resourceServerIdentifier1}/{scope1} {resourceServerIdentifier2}/{scope2}
```

**Sample response**

```
HTTP/1.1 200 OK
                            Content-Type: application/json
                            
                            {
                            "access_token":"eyJra1example", 
                            "token_type":"Bearer", 
                            "expires_in":3600
                            }
```

#### Exchanging an authorization code grant with PKCE for tokens<a name="post-token-positive-exchanging-authorization-code-grant-with-pkce-for-tokens"></a>

**Sample request**

```
POST https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/token
                                Content-Type='application/x-www-form-urlencoded'&
                                Authorization=Basic ZGpjOTh1M2ppZWRtaTI4M2V1OTI4OmFiY2RlZjAxMjM0NTY3ODkw
                                
                                grant_type=authorization_code&
                                client_id=1example23456789&
                                code=AUTHORIZATION_CODE&
                                code_verifier=CODE_VERIFIER&
                                redirect_uri=com.myclientapp://myclient/redirect
```

**Sample response**

```
HTTP/1.1 200 OK
                            Content-Type: application/json
                            
                            {
                            "access_token":"eyJra1example", 
                            "id_token":"eyJra2example",
                            "refresh_token":"eyJj3example",
                            "token_type":"Bearer", 
                            "expires_in":3600
                            }
```

**Note**  
The token endpoint returns `refresh_token` only when the `grant_type` is `authorization_code`\.

#### Exchanging a refresh token for tokens<a name="post-token-positive-exchanging-a-refresh-token-for-tokens.title"></a>

**Sample request**

```
POST https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/token >
                           Content-Type='application/x-www-form-urlencoded'&
                           Authorization=Basic ZGpjOTh1M2ppZWRtaTI4M2V1OTI4OmFiY2RlZjAxMjM0NTY3ODkw
                           
                           grant_type=refresh_token&
                           client_id=1example23456789&
                           refresh_token=eyJj3example
```

**Sample response**

```
HTTP/1.1 200 OK
                           Content-Type: application/json
                           
                           {
                           "access_token":"eyJra1example", 
                           "id_token":"eyJra2example",
                           "token_type":"Bearer", 
                           "expires_in":3600
                                 }
```

**Note**  
The token endpoint returns `refresh_token` only when the `grant_type` is `authorization_code`\.

#### Examples of negative responses<a name="post-token-negative"></a>

**Sample error response**

```
HTTP/1.1 400 Bad Request
                           Content-Type: application/json;charset=UTF-8
                           
                           {
                           "error":"invalid_request|invalid_client|invalid_grant|unauthorized_client|unsupported_grant_type"
                           }
```

*invalid\_request*  
The request is missing a required parameter, includes an unsupported parameter value \(other than `unsupported_grant_type`\), or is otherwise malformed\. For example, `grant_type` is `refresh_token` but `refresh_token` is not included\. 

*invalid\_client*  
Client authentication failed\. For example, when the client includes `client_id` and `client_secret` in the authorization header, but there's no such client with that `client_id` and `client_secret`\. 

*invalid\_grant*  
Refresh token has been revoked\.   
Authorization code has been consumed already or does not exist\.   
App client doesn't have read access to all [attributes](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-settings-attributes.html) in the requested scope\. For example, your app requests the `email` scope and your app client can read the `email` attribute, but not `email_verified`\.

*unauthorized\_client*  
Client is not allowed for code grant flow or for refreshing tokens\. 

*unsupported\_grant\_type*  
Returned if `grant_type` is anything other than `authorization_code` or `refresh_token` or `client_credentials`\. 