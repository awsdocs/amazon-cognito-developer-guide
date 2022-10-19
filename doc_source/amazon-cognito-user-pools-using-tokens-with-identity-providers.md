# Using tokens with user pools<a name="amazon-cognito-user-pools-using-tokens-with-identity-providers"></a>

Authenticate users and grant access to resources with tokens\. Tokens have claims, which are pieces of information about the user\. The ID token contains claims about the identity of the authenticated user, such as name and email\. The access token contains claims about the authenticated user, a list of the user's groups, and a list of scopes\. 

Amazon Cognito also has tokens that you can use to get new tokens or revoke existing tokens\. [Refresh a token](amazon-cognito-user-pools-using-the-refresh-token.md) to retrieve a new ID and access tokens\. [Revoke a token](amazon-cognito-user-pools-using-the-refresh-token.md#amazon-cognito-identity-user-pools-revoking-all-tokens-for-user) to revoke user access that is allowed by refresh tokens\.

Amazon Cognito issues tokens as Base64\-encoded strings\. You can decode any Amazon Cognito ID or access token from Base64 to plaintext JSON\. Amazon Cognito refresh tokens are encrypted, and can't be read by Amazon Cognito administrators or users\.

**Authenticating with tokens**  
When a user signs into your app, Amazon Cognito verifies the login information\. If the login is successful, Amazon Cognito creates a session and returns an ID, access, and refresh token for the authenticated user\. You can use the tokens to grant your users access to your own server\-side resources or to the Amazon API Gateway\. Or you can exchange them for temporary AWS credentials to access other AWS services\.

![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Storing tokens**  
Your app must be able to store tokens of varying sizes\. Token size can change for reasons including, but not limited to, additional claims, changes in encoding algorithms, and changes in encryption algorithms\. When you enable token revocation in your user pool, Amazon Cognito adds additional claims to JSON web tokens, increasing their size\. The new claims `origin_jti` and `jti` are added to access and ID tokens\. For more information about token revocation, see [Revoking tokens](https://docs.aws.amazon.com/cognito/latest/developerguide/token-revocation.html)\.

**Important**  
Best practice is to secure all tokens in transit and storage in the context of your application\. Tokens can contain personally\-identifying information about your users, and information about the security model that you use for your user pool\.

**Topics**
+ [Using the ID token](amazon-cognito-user-pools-using-the-id-token.md)
+ [Using the access token](amazon-cognito-user-pools-using-the-access-token.md)
+ [Using the refresh token](amazon-cognito-user-pools-using-the-refresh-token.md)
+ [Revoking tokens](token-revocation.md)
+ [Verifying a JSON web token](amazon-cognito-user-pools-using-tokens-verifying-a-jwt.md)
+ [Caching tokens](amazon-cognito-user-pools-using-tokens-caching-tokens.md)