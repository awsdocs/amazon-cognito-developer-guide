# UserInfo endpoint<a name="userinfo-endpoint"></a>

The `/oauth2/userInfo` endpoint returns information about the authenticated user\.

## GET /oauth2/userInfo<a name="get-userinfo"></a>

The user pool client makes requests to this endpoint directly and not through a browser\.

For more information, see [UserInfo Endpoint](http://openid.net/specs/openid-connect-core-1_0.html#UserInfo) in the OpenID Connect \(OIDC\) specification\.

**Topics**
+ [GET /oauth2/userInfo](#get-userinfo)
+ [Request parameters in header](#get-userinfo-request-header-parameters)
+ [Sample request](#get-userinfo-positive-exchanging-authorization-code-for-userinfo-sample-request)
+ [Sample positive response](#get-userinfo-response-sample)
+ [Sample negative responses](#get-userinfo-negative)

## Request parameters in header<a name="get-userinfo-request-header-parameters"></a>

*Authorization: Bearer *<access\_token>**  
Pass the access token using the authorization header field\.  
Required\.

## Sample request<a name="get-userinfo-positive-exchanging-authorization-code-for-userinfo-sample-request"></a>

```
GET https://<your-user-pool-domain>/oauth2/userInfo
                Authorization: Bearer <access_token>
```

## Sample positive response<a name="get-userinfo-response-sample"></a>

```
HTTP/1.1 200 OK
                       Content-Type: application/json;charset=UTF-8
                       {
                          "sub": "248289761001",
                          "name": "Jane Doe",
                          "given_name": "Jane",
                          "family_name": "Doe",
                          "preferred_username": "j.doe",
                          "email": "janedoe@example.com"
                       }
```

For a list of OIDC claims, see [Standard Claims](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\.

## Sample negative responses<a name="get-userinfo-negative"></a>

### Invalid request<a name="get-userinfo-negative-400"></a>

```
HTTP/1.1 400 Bad Request
                WWW-Authenticate: error="invalid_request",
                  error_description="Bad OAuth2 request at UserInfo Endpoint"
```

*invalid\_request*  
The request is missing a required parameter, includes an unsupported parameter value, or is otherwise malformed\.

### Invalid token<a name="get-userinfo-negative-401"></a>

```
HTTP/1.1 401 Unauthorized
                WWW-Authenticate: error="invalid_token",
                  error_description="Access token is expired, disabled, or deleted, or the user has globally signed out."
```

*invalid\_token*  
The access token is expired, revoked, malformed, or invalid\.