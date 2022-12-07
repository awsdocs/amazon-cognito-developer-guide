# Logout endpoint<a name="logout-endpoint"></a>

The `/logout` endpoint signs the user out and redirects either to an authorized sign\-out URL for your app client, or to the `/login` endpoint\.

## GET /logout<a name="get-logout"></a>

The `/logout` endpoint only supports `HTTPS GET`\. The user pool client typically makes this request through the system browser\. The browser is typically Custom Chrome Tab in Android or Safari View Control in iOS\.

### Request parameters<a name="get-logout-request-parameters"></a>

*client\_id*  
The app client ID for your app\. To get an app client ID, you must register the app in the user pool\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.  
Required\.

*logout\_uri*  
Redirect your user to a custom sign\-out page with a *logout\_uri* parameter\. Set its value to the app client **sign\-out URL** where you want to redirect your user after they sign out\. Use *logout\_uri* only with a *client\_id* parameter\. For more information, see [Configuring a user pool app client](cognito-user-pools-app-idp-settings.md)\.  
You can also use the *logout\_uri* parameter to redirect your user to the sign\-in page for another app client\. Set the sign\-in page for the other app client as an **Allowed callback URL** in your app client\. In your request to the `/logout` endpoint, set the value of the *logout\_uri* parameter to the URL\-encoded sign\-in page\.  
Amazon Cognito requires either a *logout\_uri* or a *redirect\_uri* parameter in your request to the `/logout` endpoint\. A *logout\_uri* parameter redirects your user to another website\.

**redirect\_uri**  
Redirect your user to your sign\-in page to authenticate with a *redirect\_uri* parameter\. Set its value to the app client **Allowed callback URL** where you want to redirect your user after they sign in again\. Add *client\_id*, *scope*, *state*, and *response\_type* parameters that you want to pass to your `/login` endpoint\.  
Amazon Cognito requires either a *logout\_uri* or a *redirect\_uri* parameter in your request to the `/logout` endpoint\. When you want to redirect your user to your `/login` endpoint to reauthenticate and pass tokens to your app, add a *redirect\_uri* parameter\.

*response\_type*  
The OAuth 2\.0 response that you want to receive from Amazon Cognito after your user signs in\. `code` and `token` are the valid values for the *response\_type* parameter\.  
Required if you use a *redirect\_uri* parameter\.

*state*  
When your app adds a *state* parameter to a request, Amazon Cognito returns its value to your app when the `/oauth2/logout` endpoint redirects your user\.  
Add this value to your requests to guard against [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks\.  
You can't set the value of a `state` parameter to a URL\-encoded JSON string\. To pass a string that matches this format in a `state` parameter, encode the string to Base64, then decode it in your app\.  
Strongly recommended if you use a *redirect\_uri* parameter\.

*scope*  
The OAuth 2\.0 scopes that you want to request from Amazon Cognito after you sign them out with a *redirect\_uri* parameter\. Amazon Cognito redirects your user to the `/login` endpoint with the *scope* parameter in your request to the `/logout` endpoint\.  
Optional if you use a *redirect\_uri* parameter\. If you don't include a *scope* parameter, Amazon Cognito redirects your user to the `/login` endpoint with a *scope* parameter\. When Amazon Cognito redirects your user and automatically populates `scope`, the parameter includes all authorized scopes for your app client\.

### Sample requests<a name="get-logout-request-sample"></a>

**Example 1: Log out and redirect user to client**

This example clears the existing session and redirects the user to the client\. An example request like this one, with a `logout_uri` parameter, also requires a `client_id` parameter\.

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/logout?

client_id=ad398u21ijw3s9w3939&
logout_uri=https://myclient/logout
```

**Example 2: Log out and prompt the user to sign in as another user**

This example clears the existing session and shows the login screen\. The example uses the same parameters as you would use in a request to the [Authorize endpoint](authorization-endpoint.md)\.

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/logout?

response_type=code&
client_id=ad398u21ijw3s9w3939&
redirect_uri=https://YOUR_APP/redirect_uri&
state=STATE&
scope=openid+profile+aws.cognito.signin.user.admin
```
