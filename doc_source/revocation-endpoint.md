# REVOCATION endpoint<a name="revocation-endpoint"></a>

The /`oauth2/revoke` endpoint revokes all of the access tokens that the specified refresh token generated\. After the endpoint revokes the tokens, you can't use the revoked tokens to access APIs that Amazon Cognito tokens authenticate\.

## POST /oauth2/revoke<a name="post-revoke"></a>

The `/oauth2/revoke` endpoint only supports `HTTPS POST`\. The user pool client makes requests to this endpoint directly and not through the system browser\.

### Request parameters in header<a name="revocation-request-parameters"></a>

*Authorization*  
If your app client has a client secret, the app must pass its `client_id` and `client_secret` in the authorization header through Basic HTTP authorization\. The secret is [Basic](https://en.wikipedia.org/wiki/Basic_access_authentication#Client_side) `Base64Encode(client_id:client_secret)`\.

*Content\-Type*  
Must always be `'application/x-www-form-urlencoded'`\.

#### Request parameters in body<a name="revocation-request-parameters-body"></a>

*token*  
The refresh token that the client wants to revoke\. The request also revokes all access tokens that Amazon Cognito issued from this refresh token\.  
Required\.

*client\_id*  
The app client ID for the token that you want to revoke\.  
Required if the client is public and doesn't have a secret\.

##### Revocation request examples<a name="revoke-sample-request"></a>

**Example \#1: Revoke a token for an app client without a client secret**

```
    POST /oauth2/revoke HTTP/1.1
        Host: https://mydomain.auth.us-east-1.amazoncognito.com
        Accept: application/json
        Content-Type: application/x-www-form-urlencoded
        token=2YotnFZFEjr1zCsicMWpAA&
        client_id=djc98u3jiedmi283eu928
```

**Example \#2: Revoke a token for an app client with a client secret**

```
    POST /oauth2/revoke HTTP/1.1
        Host: https://mydomain.auth.us-east-1.amazoncognito.com
        Accept: application/json
        Content-Type: application/x-www-form-urlencoded
        Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
        token=2YotnFZFEjr1zCsicMWpAA
```

##### Revocation error response<a name="revoke-sample-response"></a>

A successful response contains an empty body\. The error response is a JSON object with an `error` field and, in some cases, an `error_description` field\.

**Endpoint errors**
+ If the token isn't present in the request or if the feature is disabled for the app client, you receive an HTTP 400 and error `invalid_request`\.
+ If the token that Amazon Cognito sent in the revocation request isn't a refresh token, you receive an HTTP 400 and error `unsupported_token_type`\.
+ If the client credentials aren't valid, you receive an HTTP 401 and error `invalid_client`\.
+ If the token has been revoked or if the client submitted a token that isn't valid, you receive an HTTP 200 OK\. 