# Using tokens with user pools<a name="amazon-cognito-user-pools-using-tokens-with-identity-providers"></a>

Authenticate users and grant access to resources with tokens\. Tokens have claims, which are pieces of information about the user\. The ID token contains claims about the identity of the authenticated user, such as name and email\. The Access token contains claims about the authenticated user, a list of the user's groups, and a list of scopes\. 

Amazon Cognito also has tokens that you can use to get new tokens or revoke existing tokens\. [Refresh a token](amazon-cognito-user-pools-using-the-refresh-token.md) to retrieve a new ID and access tokens\. [Revoke a token](amazon-cognito-user-pools-using-the-refresh-token.md#amazon-cognito-identity-user-pools-revoking-all-tokens-for-user) to revoke user access that is allowed by refresh tokens\.

**Authenticating with tokens**  
When a user signs into your app, Amazon Cognito verifies the login information\. If the login is successful, Amazon Cognito creates a session and returns an ID, access, and refresh token for the authenticated user\. You can use the tokens to grant your users access to your own server\-side resources or to the Amazon API Gateway\. Or you can exchange them for temporary AWS credentials to access other AWS services\.

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Important**  
We strongly recommended that you secure all tokens in transit and storage in the context of your application\.

**Topics**
+ [Using the ID token](amazon-cognito-user-pools-using-the-id-token.md)
+ [Using the access token](amazon-cognito-user-pools-using-the-access-token.md)
+ [Using the refresh token](amazon-cognito-user-pools-using-the-refresh-token.md)
+ [Revoking tokens](token-revocation.md)
+ [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)