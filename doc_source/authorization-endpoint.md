# AUTHORIZATION Endpoint<a name="authorization-endpoint"></a>

The `/oauth2/authorize` endpoint signs the user in\.

## GET /oauth2/authorize<a name="get-authorize"></a>

The `/oauth2/authorize` endpoint only supports `HTTPS GET`\. The user pool client typically makes this request through a browser\. Web browsers include Chrome or Firefox\. Android browsers include Custom Chrome Tab\. iOS browsers include Safari View Control\.

The authorization server requires HTTPS instead of HTTP as the protocol when accessing the authorization endpoint\. For more information on the specification see [Authorization Endpoint](http://openid.net/specs/openid-connect-core-1_0.html#ImplicitAuthorizationEndpoint)\.

### Request Parameters<a name="get-authorize-request-parameters"></a>

*response\_type*  
The response type\. Must be `code` or `token`\. Indicates whether the client wants an authorization code \(authorization code grant flow\) for the end user or directly issues tokens for end user \(implicit flow\)\.   
Required

*client\_id*  
The Client ID\.  
Must be a pre\-registered client in the user pool and must be enabled for federation\.  
Required

*redirect\_uri*  
The URL to which the authentication server redirects the browser after authorization has been granted by the user\.  
A redirect URI must:  
+ Be an absolute URI\.
+ Be pre\-registered with a client\.
+ Not include a fragment component\.
See [OAuth 2\.0 \- Redirection Endpoint](https://tools.ietf.org/html/rfc6749#section-3.1.2)\.  
Amazon Cognito requires `HTTPS` over `HTTP` except for `http://localhost` for testing purposes only\.  
App callback URLs such as `myapp://example` are also supported\.  
Required

*state*  
An opaque value the clients adds to the initial request\. The authorization server includes this value when redirecting back to the client\.  
This value must be used by the client to prevent [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks\.  
Optional but strongly recommended\.

*identity\_provider*  
Used by the developer to directly authenticate with a specific provider\.  
+ For social sign\-in the valid values are **Facebook**, **Google**, **LoginWithAmazon**, and **SignInWithApple**\.
+ For Amazon Cognito user pools, the value is **COGNITO**\.
+ For other identity providers this would be the name you assigned to the IdP in your user pool\.
Optional

*idp\_identifier*  
Used by the developer to map to a provider name without exposing the provider name\.  
Optional

*scope*  
Can be a combination of any system\-reserved scopes or custom scopes associated with a client\. Scopes must be separated by spaces\. System reserved scopes are `openid`, `email`, `phone`, `profile`, and `aws.cognito.signin.user.admin`\. Any scope used must be preassociated with the client or it will be ignored at runtime\.  
If the client doesn't request any scopes, the authentication server uses all scopes associated with the client\.  
An ID token is only returned if `openid` scope is requested\. The access token can be only used against Amazon Cognito User Pools if `aws.cognito.signin.user.admin` scope is requested\. The `phone`, `email`, and `profile` scopes can only be requested if `openid` scope is also requested\. These scopes dictate the claims that go inside the ID token\.  
Optional

*code\_challenge\_method*  
The method used to generate the challenge\. The [PKCE RFC](https://tools.ietf.org/html/rfc7636) defines two methods, S256 and plain; however, Amazon Cognito authentication server supports only S256\.  
Optional

*code\_challenge*  
The generated challenge from the `code_verifier`\.  
Required only when the `code_challenge_method` is specified\.

### Examples Requests with Positive Responses<a name="get-authorize-positive"></a>

#### Authorization Code Grant<a name="sample-authorization-code-grant"></a>

**Sample Request**

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize?
response_type=code&
client_id=ad398u21ijw3s9w3939&
redirect_uri=https://YOUR_APP/redirect_uri&
state=STATE&
scope=openid+profile+aws.cognito.signin.user.admin
```

**Sample Response**  
The Amazon Cognito authentication server redirects back to your app with the authorization code and state\. The code and state must be returned in the query string parameters and not in the fragment\. A query string is the part of a web request that appears after a '?' character; the string can contain one or more parameters separated by '&' characters\. A fragment is the part of a web request that appears after a '\#' character to specify a subsection of a document\.

**Note**  
The response returns a one time use code that is valid for five minutes\.

```
HTTP/1.1 302 Found
Location: https://YOUR_APP/redirect_uri?code=AUTHORIZATION_CODE&state=STATE
```

#### Authorization Code Grant with PKCE<a name="sample-authorization-code-grant-with-pkce"></a>

**Sample Request**

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize?
response_type=code&
client_id=ad398u21ijw3s9w3939&
redirect_uri=https://YOUR_APP/redirect_uri&
state=STATE&
scope=aws.cognito.signin.user.admin&
code_challenge_method=S256&
code_challenge=CODE_CHALLENGE
```

**Sample Response**  
The authentication server redirects back to your app with the authorization code and state\. The code and state must be returned in the query string parameters and not in the fragment\.

```
HTTP/1.1 302 Found
Location: https://YOUR_APP/redirect_uri?code=AUTHORIZATION_CODE&state=STATE
```

#### Token grant without `openid` scope<a name="sample-token-grant-without-openid-scope"></a>

**Sample Request**

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize?
response_type=token&
client_id=ad398u21ijw3s9w3939&
redirect_uri=https://YOUR_APP/redirect_uri&
state=STATE&
scope=aws.cognito.signin.user.admin
```

**Sample Response**  
The Amazon Cognito authorization server redirects back to your app with access token\. Since `openid` scope was not requested, an ID token is not returned\. A refresh token is never returned in this flow\. Token and state are returned in the fragment and not in the query string\.

```
HTTP/1.1 302 Found
Location: https://YOUR_APP/redirect_uri#access_token=ACCESS_TOKEN&token_type=bearer&expires_in=3600&state=STATE
```

#### Token grant with `openid` scope<a name="sample-token-grant-with-openid-scope"></a>

**Sample Request**

```
GET
                            https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize? 
response_type=token& 
client_id=ad398u21ijw3s9w3939& 
redirect_uri=https://YOUR_APP/redirect_uri& 
state=STATE&
scope=aws.cognito.signin.user.admin+openid+profile
```

**Sample Response**  
The authorization server redirects back to your app with access token and ID token \(because `openid` scope was included\)\.

```
HTTP/1.1 302 Found
Location: https://YOUR_APP/redirect_ur#id_token=ID_TOKEN&access_token=ACCESS_TOKEN&token_type=bearer&expires_in=3600&state=STATE
```

### Examples of Negative Responses<a name="get-authorize-negative"></a>

The following are examples of negative responses:
+ If `client_id` and `redirect_uri` are valid but there are other problems with the request parameters \(for example, if `response_type` is not included; if `code_challenge` is supplied but `code_challenge_method` is not supplied; or if `code_challenge_method` is not 'S256'\), the authentication server redirects the error to client's `redirect_uri`\. 

   ` HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request `
+ If the client requests 'code' or 'token' in `response_type` but does not have permission for these requests, the Amazon Cognito authorization server should return `unauthorized_client` to client's `redirect_uri`, as follows: 

  `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=unauthorized_client`
+  If the client requests invalid, unknown, malformed scope, the Amazon Cognito authorization server should return `invalid_scope` to the client's `redirect_uri`, as follows: 

  `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_scope`
+ If there is any unexpected error in the server, the authentication server should return `server_error` to client's `redirect_uri`\. It should not be the HTTP 500 error displayed to the end user in the browser, because this error doesn't get sent to the client\. The following error should return:

  `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=server_error` 
+ When authenticating by federating to third\-party identity providers, Cognito may experience connection issues such as the following:
  + If a connection timeout occurs while requesting token from the identity provider, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Timeout+occurred+in+calling+IdP+token+endpoint`
  + If a connection timeout occurs while calling jwks endpoint for id\_token validation, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=error_description=Timeout+in+calling+jwks+uri`
+ When authenticating by federating to third\-party identity providers, the providers may return error responses due to configuration errors or otherwise such as the following:
  + If an error response is received from other providers, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=[IdP name]+Error+-+[status code]+error getting token`
  + If an error response is received from Google, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Google+Error+-+[status code]+[Google provided error code]`
+ In the rare case where Cognito encounters an exception in the communication protocol while making any connection to an external identity provider, the authentication server redirects the error to the client's `redirect_uri` with either of the following messages:
  + `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Connection+reset`
  + `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Read+timed+out`