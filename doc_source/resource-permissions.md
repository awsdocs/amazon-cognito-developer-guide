# Identity and Access Management in Amazon Cognito user pools<a name="resource-permissions"></a>

This section covers restricting access to Amazon Cognito resources via AWS Identity and Access Management \(IAM\)\. For more information about using identity pools together with user pool groups to control access your AWS resources see [Adding Groups to a User Pool](cognito-user-pools-user-groups.md) and [Role\-Based Access Control](role-based-access-control.md)\. See [Identity Pools Concepts \(Federated Identities\) ](concepts.md) for more information about identity pools and IAM\.

## Amazon Resource Names \(ARNs\)<a name="amazon-cognito-amazon-resource-names"></a>

**ARNs for Amazon Cognito Federated Identities**

In Amazon Cognito identity pools \(federated identities\), it is possible to restrict an IAM user's access to a specific identity pool, using the Amazon Resource Name \(ARN\) format, as in the following example\. For more information about ARNs, see [IAM Identifiers](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html)\.

```
arn:aws:cognito-identity:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID
```

**ARNs for Amazon Cognito Sync**

In Amazon Cognito Sync, customers can also restrict access by the identity pool ID, identity ID, and dataset name\.

For APIs that operate on an identity pool, the identity pool ARN format is the same as for Amazon Cognito Federated Identities, except that the service name is `cognito-sync` instead of `cognito-identity`:

```
arn:aws:cognito-sync:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID
```

For APIs that operate on a single identity, such as `RegisterDevice`, you can refer to the individual identity by the following ARN format:

```
arn:aws:cognito-sync:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID/identity/IDENTITY_ID
```

For APIs that operate on datasets, such as `UpdateRecords` and `ListRecords`, you can refer to the individual dataset using the following ARN format:

```
arn:aws:cognito-sync:REGION:ACCOUNT_ID:identitypool/IDENTITY_POOL_ID/identity/IDENTITY_ID/dataset/DATASET_NAME
```

**ARNs for Amazon Cognito Your User Pools**

For Amazon Cognito Your User Pools, it is possible to restrict an IAM user's access to a specific user pool, using the following ARN format:

```
arn:aws:cognito-idp:REGION:ACCOUNT_ID:userpool/USER_POOL_ID
```

## Example Policies<a name="amazon-cognito-example-policies"></a>

**Restricting Console Access to a Specific Identity Pool**

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cognito-identity:ListIdentityPools"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cognito-identity:*"
      ],
      "Resource": "arn:aws:cognito-identity:us-east-1:0123456789:identitypool/us-east-1:1a1a1a1a-ffff-1111-9999-12345678"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cognito-sync:*"
      ],
      "Resource": "arn:aws:cognito-sync:us-east-1:0123456789:identitypool/us-east-1:1a1a1a1a-ffff-1111-9999-12345678"
    }
  ]
}
```

**Allowing Access to Specific Dataset for All Identities in a Pool**

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cognito-sync:ListRecords",
        "cognito-sync:UpdateRecords"
      ],
      "Resource": "arn:aws:cognito-sync:us-east-1:0123456789:identitypool/us-east-1:1a1a1a1a-ffff-1111-9999-12345678/identity/*/dataset/UserProfile"
    }
  ]
}
```

## Managed Policies<a name="amazon-cognito-managed-policies"></a>

A number of policies are available via the IAM Console that customers can use to grant access to Amazon Cognito:
+ AmazonCognitoPowerUser \- Permissions for accessing and managing all aspects of your identity pools\.
+ AmazonCognitoReadOnly \- Permissions for read only access to your identity pools\.
+ AmazonCognitoDeveloperAuthenticatedIdentities \- Permissions for your authentication system to integrate with Amazon Cognito\.

These policies are maintained by the Amazon Cognito team, so even as new APIs are added your IAM users will continue to have the same level of access\.

**Note**  
Because creating a new identity pool also requires creating IAM roles, any IAM user you want to be able to create new identity pools with must have the admin policy applied as well\.

## Signed versus Unsigned APIs<a name="amazon-cognito-signed-versus-unsigned-apis"></a>

APIs that are signed with AWS credentials are capable of being restricted via an IAM policy\. The following Cognito APIs are unsigned, and therefore cannot be restricted via an IAM policy:

**Amazon Cognito Federated Identities**
+ `GetId`
+ `GetOpenIdToken`
+ `GetCredentialsForIdentity`
+ `UnlinkIdentity`

**Amazon Cognito Your User Pools**
+ `ChangePassword`
+ `ConfirmDevice`
+ `ConfirmForgotPassword`
+ `ConfirmSignUp`
+ `DeleteUser`
+ `DeleteUserAttributes`
+ `ForgetDevice`
+ `ForgotPassword`
+ `GetDevice`
+ `GetUser`
+ `GetUserAttributeVerificationCode`
+ `GlobalSignOut`
+ `InitiateAuth`
+ `ListDevices`
+ `ResendConfirmationCode`
+ `RespondToAuthChallenge`
+ `SetUserSettings`
+ `SignUp`
+ `UpdateDeviceStatus`
+ `UpdateUserAttributes`
+ `VerifyUserAttribute`

**Topics**
+ [Amazon Resource Names \(ARNs\)](#amazon-cognito-amazon-resource-names)
+ [Example Policies](#amazon-cognito-example-policies)
+ [Managed Policies](#amazon-cognito-managed-policies)
+ [Signed versus Unsigned APIs](#amazon-cognito-signed-versus-unsigned-apis)
+ [Using Service\-Linked Roles for Amazon Cognito](using-service-linked-roles.md)
+ [Authentication with a User Pool](authentication.md)