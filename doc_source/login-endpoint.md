# Login endpoint<a name="login-endpoint"></a>

The `/login` endpoint signs the user in\. It loads the login page and presents the authentication options configured for the client to the user\.

## GET /login<a name="get-login"></a>

The `/login` endpoint only supports `HTTPS GET`\. The user pool client makes this request through a system browser\. System browsers for JavaScript include Chrome or Firefox\. Android browsers include Custom Chrome Tab\. iOS browsers include Safari View Control\.

### Request parameters<a name="get-login-request-parameters"></a>

*client\_id*  
The app client ID for your app\. To obtain an app client ID, register the app in the user pool\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.  
Required\.

*redirect\_uri*  
The URI where the user is redirected after a successful authentication\. It should be configured on `response_type` of the specified `client_id`\.  
Required\.

*response\_type*  
The OAuth response type, which can be `code` for code grant flow and `token` for implicit flow\.  
Required\.

*state*  
When your app adds a *state* parameter to a request, Amazon Cognito returns its value to your app when the `/oauth2/login` endpoint redirects your user\.  
Add this value to your requests to guard against [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks\.  
You can't set the value of a `state` parameter to a URL\-encoded JSON string\. To pass a string that matches this format in a `state` parameter, encode the string to Base64, then decode it in your app\.  
Optional but recommended\.

*scope*  
Can be a combination of any system\-reserved scopes or custom scopes associated with a client\. Scopes must be separated by spaces\. System reserved scopes are `openid`, `email`, `phone`, `profile`, and `aws.cognito.signin.user.admin`\. Any scope that you request must be activated for the app client, or Amazon Cognito will ignore it\.  
If the client doesn't request any scopes, the authentication server uses all scopes associated with the client\.  
An ID token is only returned if an `openid` scope is requested\. The access token can only be used against Amazon Cognito user pools if an `aws.cognito.signin.user.admin` scope is requested\. The `phone`, `email`, and `profile` scopes can only be requested if an `openid` scope is also requested\. These scopes dictate the claims that go inside the ID token\.  
Optional\.

*code\_challenge\_method*  
The method used to generate the challenge\. The [PKCE RFC](https://tools.ietf.org/html/rfc7636) defines two methods, S256 and plain; however, Amazon Cognito authentication server supports only S256\.  
Optional\.

*code\_challenge*  
The generated challenge from the `code_verifier`\.  
Required only when the `code_challenge_method` is specified\.

### <a name="get-login-request-sample"></a>

**Sample request: Prompt the user to sign in**

This example displays the login screen\.

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/login?
                response_type=code&
                client_id=ad398u21ijw3s9w3939&
                redirect_uri=https://YOUR_APP/redirect_uri&
                state=STATE&
                scope=openid+profile+aws.cognito.signin.user.admin
```

**Sample Response**  
The authentication server redirects to your app with the authorization code and state\. The server must return the code and state in the query string parameters and not in the fragment\.

```
HTTP/1.1 302 Found
                    Location: https://YOUR_APP/redirect_uri?code=AUTHORIZATION_CODE&state=STATE
```

For more example requests, and examples of positive and negative responses, see [Authorize endpoint](authorization-endpoint.md)\.