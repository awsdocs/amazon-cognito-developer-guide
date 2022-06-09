# Authorize endpoint<a name="authorization-endpoint"></a>

The `/oauth2/authorize` endpoint signs in the user\. Its function is similar to the [Login endpoint](login-endpoint.md)\.

## GET `/oauth2/authorize`<a name="get-authorize"></a>

The `/oauth2/authorize` endpoint only supports `HTTPS GET`\. The user pool client typically makes this request through a browser\.

The authorization server requires HTTPS\. For more information about the OpenID Connect specification, see [Authorization Endpoint](http://openid.net/specs/openid-connect-core-1_0.html#ImplicitAuthorizationEndpoint)\.

### Request parameters<a name="get-authorize-request-parameters"></a>

*response\_type*  
The response type\. Must be `code` or `token`\. Indicates whether the client wants an authorization code for the user \(authorization code grant flow\), or directly issues tokens for the user \(implicit flow\)\.   
Required\.

*client\_id*  
The Client ID\.  
Must be a client that you already registered in the user pool and that you qualified for federation\.  
Required\.

*redirect\_uri*  
The URL where the authentication server redirects the browser after Amazon Cognito authorizes the user\.  
A redirect uniform resource identifier \(URI\) must have the following attributes:  
+ It must be an absolute URI\.
+ You must have pre\-registered the URI with a client\.
+ It can't include a fragment component\.
See [OAuth 2\.0 \- Redirection Endpoint](https://tools.ietf.org/html/rfc6749#section-3.1.2)\.  
Amazon Cognito requires that your redirect URI use HTTPS, except for `http://localhost`, which you can set as a callback URL for testing purposes\.  
Amazon Cognito also supports app callback URLs such as `myapp://example`\.  
Required\.

*state*  
When your app adds a *state* parameter to a request, Amazon Cognito returns its value to your app when the `/oauth2/authorize` endpoint redirects your user\.  
Add this value to your requests to guard against [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks\.  
You can't set the value of a `state` parameter to a URL\-encoded JSON string\. To pass a string that matches this format in a `state` parameter, encode the string to Base64, then decode it in your app\.  
Optional but recommended\.

*identity\_provider*  
Add this parameter to bypass the hosted UI and redirect your user to a provider sign\-in page\. The value of the *identity\_provider* parameter is the name of the identity provider \(IdP\) as it appears in your user pool\.  
+ For social providers, you can use the *identity\_provider* values **Facebook**, **Google**, **LoginWithAmazon**, and **SignInWithApple**\.
+ For Amazon Cognito user pools, use the value **COGNITO**\.
+ For SAML 2\.0 and OpenID Connect \(OIDC\) identity providers \(IdPs\), use the name that you assigned to the IdP in your user pool\.
Optional\.

*idp\_identifier*  
Add this parameter to redirect to a provider with an alternative name for the *identity\_provider* name\. You can enter identifiers for your SAML 2\.0 and OIDC IdPs from the **Sign\-in experience** tab of the Amazon Cognito console\.  
Optional\.

*scope*  
Can be a combination of any system\-reserved scopes or custom scopes that are associated with a client\. Scopes must be separated by spaces\. System reserved scopes are `openid`, `email`, `phone`, `profile`, and `aws.cognito.signin.user.admin`\. Any scope used must be associated with the client, or it will be ignored at runtime\.  
If the client doesn't request any scopes, the authentication server uses all scopes that are associated with the client\.  
An ID token is only returned if `openid` scope is requested\. The access token can be only used against Amazon Cognito user pools if `aws.cognito.signin.user.admin` scope is requested\. The `phone`, `email`, and `profile` scopes can only be requested if `openid` scope is also requested\. These scopes dictate the claims that go inside the ID token\.  
Optional\.

*code\_challenge\_method*  
The method that you used to generate the challenge\. The [PKCE RFC](https://tools.ietf.org/html/rfc7636) defines two methods, S256 and plain; however, Amazon Cognito authentication server supports only S256\.  
Optional\.

*code\_challenge*  
The challenge that you generated from the `code_verifier`\.  
Required only when you specify a `code_challenge_method` parameter\.

*nonce*  
A random value that you can add to the request\. The nonce value that you provide is included in the ID token that Amazon Cognito issues\. You can use a `nonce` value to guard against replay attacks\.

### Examples requests with positive responses<a name="get-authorize-positive"></a>

#### Authorization code grant<a name="sample-authorization-code-grant"></a>

**Sample Request**

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize?
                                 response_type=code&
                                 client_id=ad398u21ijw3s9w3939&
                                 redirect_uri=https://YOUR_APP/redirect_uri&
                                 state=STATE&
                                 scope=openid+profile+aws.cognito.signin.user.admin
```

****Sample response****  
The Amazon Cognito authentication server redirects back to your app with the authorization code and state\. The code and state must be returned in the query string parameters and not in the fragment\. A query string is the part of a web request that appears after a '?' character; the string can contain one or more parameters separated by '&' characters\. A fragment is the part of a web request that appears after a '\#' character to specify a subsection of a document\.

**Note**  
The response returns a one time use code that is valid for five minutes\.

```
HTTP/1.1 302 Found
                    Location: https://YOUR_APP/redirect_uri?code=AUTHORIZATION_CODE&state=STATE
```

#### Authorization code grant with PKCE<a name="sample-authorization-code-grant-with-pkce"></a>

**Sample request**

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

****Sample response****  
The authentication server redirects back to your app with the authorization code and state\. The code and state must be returned in the query string parameters and not in the fragment\.

```
HTTP/1.1 302 Found
                    Location: https://YOUR_APP/redirect_uri?code=AUTHORIZATION_CODE&state=STATE
```

#### Token grant without `openid` scope<a name="sample-token-grant-without-openid-scope"></a>

**Sample request**

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize?
                         response_type=token&
                         client_id=ad398u21ijw3s9w3939&
                         redirect_uri=https://YOUR_APP/redirect_uri&
                         state=STATE&
                         scope=aws.cognito.signin.user.admin
```

****Sample response****  
The Amazon Cognito authorization server redirects back to your app with access token\. Because `openid` scope was not requested, Amazon Cognito doesn't return an ID token\. Also, Amazon Cognito doesn't return a refresh token in this flow\. Amazon Cognito returns the access token and state in the fragment and not in the query string\.

```
HTTP/1.1 302 Found
                        Location: https://YOUR_APP/redirect_uri#access_token=ACCESS_TOKEN&token_type=bearer&expires_in=3600&state=STATE
```

#### Token grant with `openid` scope<a name="sample-token-grant-with-openid-scope"></a>

**Sample request**

```
                        GET
                            https://mydomain.auth.us-east-1.amazoncognito.com/oauth2/authorize? 
                            response_type=token& 
                            client_id=ad398u21ijw3s9w3939& 
                            redirect_uri=https://YOUR_APP/redirect_uri& 
                            state=STATE&
                            scope=aws.cognito.signin.user.admin+openid+profile
```

****Sample response****  
The authorization server redirects back to your app with access token and ID token \(because `openid` scope was included\)\.

```
HTTP/1.1 302 Found
                         Location: https://YOUR_APP/redirect_uri#id_token=ID_TOKEN&access_token=ACCESS_TOKEN&token_type=bearer&expires_in=3600&state=STATE
```

#### Examples of negative responses<a name="get-authorize-negative"></a>

The following are examples of negative responses:
+ If `client_id` and `redirect_uri` are valid, but the request parameters aren't formatted correctly, the authentication server redirects the error to client's `redirect_uri` and appends an error message in a URL parameter\. Examples of incorrect formatting are a request doesn't include a `response_type` parameter, if the response provides `code_challenge` but not `code_challenge_method`, or that `code_challenge_method` is not 'S256'\.

   ` HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request `
+ If the client requests `code` or `token` in `response_type` but doesn't have permission for these requests, the Amazon Cognito authorization server returns `unauthorized_client` to client's `redirect_uri`, as follows: 

  `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=unauthorized_client`
+  If the client requests scope that is unknown, malformed, or not valid, the Amazon Cognito authorization server returns `invalid_scope` to the client's `redirect_uri`, as follows: 

  `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_scope`
+ If there is any unexpected error in the server, the authentication server returns `server_error` to client's `redirect_uri`\. Because the HTTP 500 error doesn't get sent to the client, don't display the error to the user in the browser\. The following error should result:

  `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=server_error` 
+ When Amazon Cognito authenticates through federation to third\-party IdPs, Amazon Cognito might experience connection issues such as the following:
  + If a connection timeout occurs while requesting token from the IdP, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Timeout+occurred+in+calling+IdP+token+endpoint`
  + If a connection timeout occurs while calling the `jwks` endpoint for `id_token` validation, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=error_description=Timeout+in+calling+jwks+uri`
+ When authenticating by federating to third\-party IdPs, the providers may return error responses due to configuration errors or otherwise such as the following:
  + If an error response is received from other providers, the authentication server redirects the error to the client’s `redirect_uri` as follows:

    `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=[IdP name]+Error+-+[status code]+error getting token`
  + If an error response is received from Google, the authentication server redirects the error to the client’s `redirect_uri` as follows: `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Google+Error+-+[status code]+[Google provided error code]`
+ When Amazon Cognito encounters an exception in the communication protocol while it makes a connection to an external IdP, the authentication server redirects the error to the client's `redirect_uri` with either of the following messages:
  + `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Connection+reset`
  + `HTTP 1.1 302 Found Location: https://client_redirect_uri?error=invalid_request&error_description=Read+timed+out`