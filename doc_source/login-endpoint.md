# LOGIN endpoint<a name="login-endpoint"></a>

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
An opaque value the client adds to the initial request\. The value is then returned back to the client upon redirect\.  
This value must be used by the client to prevent [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks\.  
Optional but strongly recommended\.

*scope*  
Can be a combination of any system\-reserved scopes or custom scopes associated with a client\. Scopes must be separated by spaces\. System reserved scopes are `openid`, `email`, `phone`, `profile`, and `aws.cognito.signin.user.admin`\. Any scope used must be preassociated with the client or it is ignored at runtime\.  
If the client doesn't request any scopes, the authentication server uses all scopes associated with the client\.  
An ID token is only returned if an `openid` scope is requested\. The access token can only be used against Amazon Cognito user pools if an `aws.cognito.signin.user.admin` scope is requested\. The `phone`, `email`, and `profile` scopes can only be requested if an `openid` scope is also requested\. These scopes dictate the claims that go inside the ID token\.  
Optional\.

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
The authentication server redirects back to your app with the authorization code and state\. The code and state must be returned in the query string parameters and not in the fragment\.

```
HTTP/1.1 302 Found
                    Location: https://YOUR_APP/redirect_uri?code=AUTHORIZATION_CODE&state=STATE
```

For more example requests, as well as example positive and negative responses, see [AUTHORIZATION endpoint](authorization-endpoint.md)\.