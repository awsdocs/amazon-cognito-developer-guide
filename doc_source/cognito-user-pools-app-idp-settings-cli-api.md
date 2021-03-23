# To Configure an App Client \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-idp-settings-cli-api"></a>

You can use the AWS CLI to update, create, describe, and delete your user pool app client\.

Replace "MyUserPoolID" and "MyAppClientID" with your user pool and app client ID values in these examples\. Likewise, your parameter values might be different than those used in these examples\.

**Note**  
Use JSON format for callback and logout URLs to prevent the CLI from treating them as remote parameter files:  
\-\-callback\-urls "\["https://example\.com"\]"  
\-\-logout\-urls "\["https://example\.com"\]"

## Updating a User Pool App Client \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-idp-settings-cli-api-update-user-pool-client"></a>

```
aws cognito-idp update-user-pool-client --user-pool-id  "MyUserPoolID" --client-id "MyAppClientID" --allowed-o-auth-flows-user-pool-client --allowed-o-auth-flows "code" "implicit" --allowed-o-auth-scopes "openid" --callback-urls "["https://example.com"]" --supported-identity-providers "["MySAMLIdP", "LoginWithAmazon"]"
```

If the command is successful, the AWS CLI returns a confirmation:

```
{
    "UserPoolClient": {
        "ClientId": "MyClientID",
        "SupportedIdentityProviders": [
            "LoginWithAmazon",
            "MySAMLIdP"
        ],
        "CallbackURLs": [
            "https://example.com"
        ],
        "AllowedOAuthScopes": [
            "openid"
        ],
        "ClientName": "Example",
        "AllowedOAuthFlows": [
            "implicit",
            "code"
        ],
        "RefreshTokenValidity": 30,
        "CreationDate": 1524628110.29,
        "AllowedOAuthFlowsUserPoolClient": true,
        "UserPoolId": "MyUserPoolID",
        "LastModifiedDate": 1530055177.553
    }
}
```

See the AWS CLI command reference for more information: [update\-user\-pool\-client](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/update-user-pool-client.html)\.

AWS API: [UpdateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html)

## Creating a User Pool App Client \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-idp-settings-cli-api-create-user-pool-client"></a>

```
aws cognito-idp create-user-pool-client --user-pool-id MyUserPoolID --client-name myApp
```

See the AWS CLI command reference for more information: [create\-user\-pool\-client](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/create-user-pool-client.html)

AWS API: [CreateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolClient.html)

## Getting Information about a User Pool App Client \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-idp-settings-cli-api-describe-user-pool-client"></a>

```
aws cognito-idp describe-user-pool-client --user-pool-id MyUserPoolID --client-id MyClientID
```

See the AWS CLI command reference for more information: [describe\-user\-pool\-client](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/describe-user-pool-client.html)\.

AWS API: [DescribeUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DescribeUserPoolClient.html)

## Listing All App Client Information in a User Pool \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-idp-settings-cli-api-list-user-pool-clients"></a>

```
aws cognito-idp list-user-pool-clients --user-pool-id "MyUserPoolID" --max-results 3
```

See the AWS CLI command reference for more information: [list\-user\-pool\-clients](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/list-user-pool-clients.html)\.

AWS API: [ListUserPoolClients](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUserPoolClients.html)

## Deleting a User Pool App Client \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-idp-settings-cli-api-delete-user-pool-client"></a>

```
aws cognito-idp delete-user-pool-client --user-pool-id "MyUserPoolID" --client-id "MyAppClientID"
```

See the AWS CLI command reference for more information: [delete\-user\-pool\-client](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/delete-user-pool-client.html)

AWS API: [DeleteUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DeleteUserPoolClient.html)