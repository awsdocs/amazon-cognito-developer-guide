# LOGOUT endpoint<a name="logout-endpoint"></a>

The `/logout` endpoint signs the user out\.

## GET /logout<a name="get-logout"></a>

The `/logout` endpoint only supports `HTTPS GET`\. The user pool client typically makes this request through the system browser, which would typically be Custom Chrome Tab in Android and Safari View Control in iOS\.

### Request parameters<a name="get-logout-request-parameters"></a>

*client\_id*  
The app client ID for your app\. To obtain an app client ID, you must register the app in the user pool\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\.  
Required\.

*logout\_uri*  
A sign\-out URL that you registered for your client app\. For more information, see [Configuring a user pool app client](cognito-user-pools-app-idp-settings.md)\.  
Optional\.

### Sample requests<a name="get-logout-request-sample"></a>

**Example \#1: Logout and redirect back to client**

This example clears out the existing session and redirects back to the client\. Both parameters are required\.

```
GET https://mydomain.auth.us-east-1.amazoncognito.com/logout?

client_id=ad398u21ijw3s9w3939&
logout_uri=https://myclient/logout
```

**Example \#2: Logout and prompt the user to sign in as another user**

This example clears out the existing session and shows the login screen, using the same parameters as for `GET /oauth2/authorize`\.

```
                    
                    GET https://mydomain.auth.us-east-1.amazoncognito.com/logout?
                        response_type=code&
                        client_id=ad398u21ijw3s9w3939&
                        redirect_uri=https://YOUR_APP/redirect_uri&
                        state=STATE&
                        scope=openid+profile+aws.cognito.signin.user.admin
```