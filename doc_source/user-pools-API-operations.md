# Using the Amazon Cognito native and OIDC APIs<a name="user-pools-API-operations"></a>

When you want to sign up, sign in, and manage users in your user pool, you have two options\. 

1. The [hosted UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-integration.html), or [https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html), is part of a package of public\-facing webpages that Amazon Cognito activates when you [choose a domain](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-assign-domain.html) for your user pool\. For a quick start with the authentication and authorization features of Amazon Cognito user pools, including pages for sign\-up, sign\-in, password management, and multi\-factor authentication \(MFA\), use the built\-in user interface of the hosted UI\.

1. The [Amazon Cognito user pools API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html), or *native API*, is a way for your app to collect information in your own custom front end, then authenticate and authorize user access with REST API requests to an [AWS service endpoint](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html)\.

The Amazon Cognito user pools API is dual\-purpose\. It creates and configures your Amazon Cognito user pools resources\. For example, you can create user pools, add AWS Lambda triggers, and configure your hosted UI domain\. The native API also performs sign\-up, sign\-in and other user operations for *native* users\. Native users are those that you created, or who signed up, directly in your user pool without a third\-party identity provider \(IdP\)\.

# Example scenario with the Amazon Cognito user pools API

1. Your user selects a "Create an account" button that you created in your app\. They enter an email address and password\.

1. Your app sends a [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html) API request and creates a new user in your user pool\.

1. Your app prompts your user for an email confirmation code\. Your users enters the code they received in an email message\.

1. Your app sends a [ConfirmSignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ConfirmSignUp.html) API request with the user's confirmation code\.

1. Your app prompts your user for their user name and password, and they enter their information\.

1. Your app sends an [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API request and stores an ID token, access token, and refresh token\. Your app calls OIDC libraries to manage your user's tokens and maintain a persistent session for that user\.

In the Amazon Cognito user pools API, you can't sign in users who federate through an IdP\. You must authenticate these users through the OIDC API\. For more information about the OIDC API endpoints that include the hosted UI, see [User pool OIDC and hosted UI API endpoints reference](cognito-userpools-server-contract-reference.md)\. Your federated users can start in the hosted UI and select their IdP, or you can skip the hosted UI and send your users directly to your IdP to sign in\. When your API request to the [Authorize endpoint](authorization-endpoint.md) includes an IdP parameter, Amazon Cognito silently redirects your user to the IdP sign\-in page\.

# Example scenario with the OIDC API

1. Your user selects a "Create an account" button that you created in your app\.

1. You present your user with a list of the social identity providers where you have registered developer credentials\. Your user chooses Apple\.

1. Your app initiates a request to the [Authorize endpoint](authorization-endpoint.md) with provider name `SignInWithApple`\.

1. Your user's browser opens the Apple OAuth authorization page\. Your user chooses to allow Amazon Cognito to read their profile information\.

1. Amazon Cognito confirms the Apple access token and queries your user's Apple profile\.

1. Your user presents an Amazon Cognito authorization code to your app\.

1. Your app exchanges the authorization code with the [Token endpoint](token-endpoint.md) and stores an ID token, access token, and refresh token\. Your app calls OIDC libraries to manage your user's tokens and maintain a persistent session for that user\.

The native API and the OIDC API support a variety of scenarios, described throughout this guide\. The following sections examine how the native API further divides into classes that support your sign\-up, sign\-in, and resource\-management requirements\.

## Amazon Cognito user pools authenticated and unauthenticated API operations<a name="user-pool-apis-auth-unauth"></a>

The Amazon Cognito user pools API, both a resource\-management interface and a user\-facing authentication & authorization interface, combines the authorization models that follow in its operations\. Depending on the API operation, you might have to provide authorization with IAM credentials, an access token, a session token, a client secret, or a combination of these\. For many user authentication and authorization operations, you have a choice of authenticated and unauthenticated versions of the request\. Unauthenticated operations are best security practice for apps that you distribute to your users, like mobile apps; you don't need to include any secrets in your code\.

You can only assign permissions in IAM policies for [IAM\-authenticated management operations](#user-pool-apis-auth-unauth-sigv4-management) and [IAM\-authenticated user operations](#user-pool-apis-auth-unauth-sigv4-user)\.

### IAM\-authenticated management operations<a name="user-pool-apis-auth-unauth-sigv4-management"></a>

IAM\-authenticated management operations modify and view your user pool and app client configuration, like you would do in the AWS Management Console\. 

For example, to modify your user pool in an [UpdateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPool.html) API request, you must present AWS credentials and IAM permissions to update the resource\.

To authorize these requests in the AWS Command Line Interface \(AWS CLI\) or an AWS SDK, configure your environment with environment variables or client configuration that adds IAM credentials to your request\. For more information, see [Accessing AWS using your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) in the *AWS General Reference*\. You can also send requests directly to the [service endpoints](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html) for the Amazon Cognito native API\. You must authorize, or *sign*, these requests with AWS credentials that you embed in the header of your request\. For more information, see [Signing AWS API requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)\.


| IAM\-authenticated management operations | 
| --- |
| AddCustomAttributes | 
| CreateGroup | 
| CreateIdentityProvider | 
| CreateResourceServer | 
| CreateUserImportJob | 
| CreateUserPool | 
| CreateUserPoolClient | 
| CreateUserPoolDomain | 
| DeleteGroup | 
| DeleteIdentityProvider | 
| DeleteResourceServer | 
| DeleteUserPool | 
| DeleteUserPoolClient | 
| DeleteUserPoolDomain | 
| DescribeIdentityProvider | 
| DescribeResourceServer | 
| DescribeRiskConfiguration | 
| DescribeUserImportJob | 
| DescribeUserPool | 
| DescribeUserPoolClient | 
| DescribeUserPoolDomain | 
| GetCSVHeader | 
| GetGroup | 
| GetIdentityProviderByIdentifier | 
| GetSigningCertificate | 
| GetUICustomization | 
| GetUserPoolMfaConfig | 
| ListGroups | 
| ListIdentityProviders | 
| ListResourceServers | 
| ListTagsForResource | 
| ListUserImportJobs | 
| ListUserPoolClients | 
| ListUserPools | 
| ListUsers | 
| ListUsersInGroup | 
| SetRiskConfiguration | 
| SetUICustomization | 
| SetUserPoolMfaConfig | 
| StartUserImportJob | 
| StopUserImportJob | 
| TagResource | 
| UntagResource | 
| UpdateGroup | 
| UpdateIdentityProvider | 
| UpdateResourceServer | 
| UpdateUserPool | 
| UpdateUserPoolClient | 
| UpdateUserPoolDomain | 

### IAM\-authenticated user operations<a name="user-pool-apis-auth-unauth-sigv4-user"></a>

IAM\-authenticated user operations sign up, sign in, manage credentials for, modify, and view your users\. 

For example, you can have a server\-side application tier that backs a web front end\. Your server\-side app is an OAuth confidential client that you trust with privileged access to your Amazon Cognito resources\. To register a user in the app, your server can include AWS credentials in an [AdminCreateUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminCreateUser.html) API request\. For more information about OAuth client types, see [Client Types](https://www.rfc-editor.org/rfc/rfc6749#section-2.1) in *The OAuth 2\.0 Authorization Framework*\.

To authorize these requests in the AWS CLI or an AWS SDK, configure your server\-side app environment with environment variables or client configuration that adds IAM credentials to your request\. For more information, see [Accessing AWS using your AWS credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) in the *AWS General Reference*\. You can also send requests directly to the [service endpoints](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html) for the Amazon Cognito native API\. You must authorize, or *sign*, these requests with AWS credentials that you embed in the header of your request\. For more information, see [Signing AWS API requests](https://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)\.

If your app client has a client secret, you must provide both your IAM credentials and, depending on the operation, either the `SecretHash` parameter or the `SECRET_HASH` value in `AuthParameters`\. For more information, see [Computing secret hash values](signing-up-users-in-your-app.md#cognito-user-pools-computing-secret-hash)\.


| IAM\-authenticated user operations | 
| --- |
| AdminAddUserToGroup | 
| AdminConfirmSignUp | 
| AdminCreateUser | 
| AdminDeleteUser | 
| AdminDeleteUserAttributes | 
| AdminDisableProviderForUser | 
| AdminDisableUser | 
| AdminEnableUser | 
| AdminForgetDevice | 
| AdminGetDevice | 
| AdminGetUser | 
| AdminInitiateAuth | 
| AdminLinkProviderForUser | 
| AdminListDevices | 
| AdminListGroupsForUser | 
| AdminListUserAuthEvents | 
| AdminRemoveUserFromGroup | 
| AdminResetUserPassword | 
| AdminRespondToAuthChallenge | 
| AdminSetUserMFAPreference | 
| AdminSetUserPassword | 
| AdminSetUserSettings | 
| AdminUpdateAuthEventFeedback | 
| AdminUpdateDeviceStatus | 
| AdminUpdateUserAttributes | 
| AdminUserGlobalSignOut | 

### Unauthenticated user operations<a name="user-pool-apis-auth-unauth-unauth"></a>

Unauthenticated user operations sign up, sign in, and initiate password resets for your users\. Use unauthenticated, or *public*, API operations when you want anyone on the internet to sign up and sign in to your app\.

For example, to register a user in your app, you can distribute an OAuth public client that doesn't provide any privileged access to secrets\. You can register this user with the unauthenticated API operation [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html)\.

To send these requests in a public client that you developed with an AWS SDK, you don't need to configure any credentials\. You can also send requests directly to the [service endpoints](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html) for the Amazon Cognito native API with no additional authorization\.

If your app client has a client secret, you must provide, depending on the operation, either the `SecretHash` parameter or the `SECRET_HASH` value in `AuthParameters`\. For more information, see [Computing secret hash values](signing-up-users-in-your-app.md#cognito-user-pools-computing-secret-hash)\.


| Unauthenticated user operations | 
| --- |
| SignUp | 
| ConfirmSignUp | 
| ResendConfirmationCode | 
| ForgotPassword | 
| ConfirmForgotPassword | 
| InitiateAuth | 

### Token\-authenticated user operations<a name="user-pool-apis-auth-unauth-token-auth"></a>

Token\-authenticated user operations sign out, manage credentials for, modify, and view your users after they have signed in or begun the sign\-in process\. Use token\-authenticated API operations when you don't want to distribute secrets in your app, and you want to authorize requests with your user's own credentials\. If your user has completed sign\-in, you must authorize their token\-authenticated API request with an access token\. If your user is in the middle of a sign\-in process, you must authorize their token\-authenticated API request with a session token that Amazon Cognito returned in the response to the previous request\.

For example, in a public client, you might want to update a user's profile in a way that restricts the write access to the user's own profile only\. To make this update, your client can include the user's access token in a [UpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html) API request\.

To send these requests in a public client that you developed with an AWS SDK, you don't need to configure any credentials\. Include an `AccessToken` or `Session` parameter in your request\. You can also send requests directly to the [service endpoints](https://docs.aws.amazon.com/general/latest/gr/cognito_identity.html) for the Amazon Cognito native API\. To authorize a request to a service endpoint, include the access or session token in the POST body of your request\.


| Token\-authenticated user operations | AccessToken | Session | 
| --- |--- |--- |
| RespondToAuthChallenge |  | ✓ | 
| ChangePassword | ✓ |  | 
| GetUser | ✓ |  | 
| UpdateUserAttributes | ✓ |  | 
| DeleteUserAttributes | ✓ |  | 
| DeleteUser | ✓ |  | 
| ConfirmDevice | ✓ |  | 
| ForgetDevice | ✓ |  | 
| GetDevice | ✓ |  | 
| ListDevices | ✓ |  | 
| UpdateDeviceStatus | ✓ |  | 
| GetUserAttributeVerificationCode | ✓ |  | 
| VerifyUserAttribute | ✓ |  | 
| SetUserSettings | ✓ |  | 
| SetUserMFAPreference | ✓ |  | 
| GlobalSignOut | ✓ |  | 
| AssociateSoftwareToken | ✓ | ✓ | 
| VerifySoftwareToken | ✓ | ✓ | 
| RevokeToken¹ |  |  | 

¹ RevokeToken takes a refresh token as a parameter\. The refresh token serves as the authorizing token, and as the target resource\.