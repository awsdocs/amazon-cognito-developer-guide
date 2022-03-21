# LOGOUT endpoint<a name="logout-endpoint"></a>

The `/logout` endpoint signs the user out\.

## GET /logout<a name="get-logout"></a>

The `/logout` endpoint only supports `HTTPS GET`\. The user pool client typically makes this request through the system browser\. The browser is typically Custom Chrome Tab in Android or Safari View Control in iOS\.

### Request parameters<a name="get-logout-request-parameters"></a>

*client\_id*  
The app client ID for your app\. To get an app client ID, you must register the app in the user pool\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.  
Required\.

*logout\_uri*  
A sign\-out URL that you registered for your client app\. For more information, see [Configuring a user pool app client](cognito-user-pools-app-idp-settings.md)\.  
Optional\.

### Sample requests<a name="get-logout-request-sample"></a>

**Example \#1: Log out and redirect user to client**

This example clears the existing session and redirects the user to the client\. An example request like this one, with a `logout_uri` parameter, also requires a `client_id` parameter\.

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/logout?

client_id=ad398u21ijw3s9w3939&
logout_uri=https://myclient/logout
```

**Example \#2: Log out and prompt the user to sign in as another user**

This example clears the existing session and shows the login screen\. The example uses the same parameters as you would use in a request to the [AUTHORIZATION endpoint](authorization-endpoint.md)\.

```
                    
                    GET https://mydomain.auth.us-east-1.amazoncognito.com/logout?
                        response_type=code&
                        client_id=ad398u21ijw3s9w3939&
                        redirect_uri=https://YOUR_APP/redirect_uri&
                        state=STATE&
                        scope=openid+profile+aws.cognito.signin.user.admin
```