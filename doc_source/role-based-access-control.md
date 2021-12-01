# Role\-based access control<a name="role-based-access-control"></a>

Amazon Cognito identity pools assign your authenticated users a set of temporary, limited privilege credentials to access your AWS resources\. The permissions for each user are controlled through [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that you create\. You can define rules to choose the role for each user based on claims in the user's ID token\. You can define a default role for authenticated users\. You can also define a separate IAM role with limited permissions for guest users who are not authenticated\.

## Creating roles for role mapping<a name="creating-roles-for-role-mapping"></a>

It is important to add the appropriate trust policy for each role so that it can only be assumed by Amazon Cognito for authenticated users in your identity pool\. Here is an example of such a trust policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Federated": "cognito-identity.amazonaws.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "cognito-identity.amazonaws.com:aud": "us-east-1:12345678-corner-cafe-123456790ab"
        },
        "ForAnyValue:StringLike": {
          "cognito-identity.amazonaws.com:amr": "authenticated"
        }
      }
    }
  ]
}
```

This policy allows federated users from `cognito-identity.amazonaws.com` \(the issuer of the OpenID Connect token\) to assume this role\. Additionally, the policy restricts the `aud` of the token, in this case the identity pool ID, to match the identity pool\. Finally, the policy specifies that the `amr` of the token contains the value `authenticated`\.

## Granting pass role permission<a name="granting-pass-role-permission"></a>

To allow an IAM user to set roles with permissions in excess of the user's existing permissions on an identity pool, you grant that user `iam:PassRole` permission to pass the role to the `set-identity-pool-roles` API\. For example, if the user cannot write to Amazon S3, but the IAM role that the user sets on the identity pool grants write permission to Amazon S3, the user can only set this role if `iam:PassRole` permission is granted for the role\. The following example policy shows how to allow `iam:PassRole` permission\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::123456789012:role/myS3WriteAccessRole"
            ]
        }
    ]
}
```

In this policy example, the `iam:PassRole` permission is granted for the `myS3WriteAccessRole` role\. The role is specified using the role's ARN\. You must also attach this policy to your IAM user or role to which your user belongs\. For more information, see [Working with Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html)\.

**Note**  
Lambda functions use resource\-based policy, where the policy is attached directly to the Lambda function itself\. When creating a rule that invokes a Lambda function, you do not pass a role, so the user creating the rule does not need the `iam:PassRole` permission\. For more information about Lambda function authorization, see [Manage Permissions: Using a Lambda Function Policy](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#intro-permission-model-access-policy)\.

## Using tokens to assign roles to users<a name="using-tokens-to-assign-roles-to-users"></a>

For users who log in through Amazon Cognito user pools, roles can be passed in the ID token that was assigned by the user pool\. The roles appear in the following claims in the ID token:
+ The `cognito:preferred_role` claim is the role ARN\.
+ The `cognito:roles` claim is a comma\-separated string containing a set of allowed role ARNs\.

The claims are set as follows:
+ The `cognito:preferred_role` claim is set to the role from the group with the best \(lowest\) `Precedence` value\. If there is only one allowed role, `cognito:preferred_role` is set to that role\. If there are multiple roles and no single role has the best precedence, this claim is not set\.
+ The `cognito:roles` claim is set if there is at least one role\.

When using tokens to assign roles, if there are multiple roles that can be assigned to the user, Amazon Cognito identity pools \(federated identities\) chooses the role as follows:
+ Use the [GetCredentialsForIdentity](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_GetCredentialsForIdentity.html) `CustomRoleArn` parameter if it is set and it matches a role in the `cognito:roles` claim\. If this parameter doesn't match a role in `cognito:roles`, deny access\.
+ If the `cognito:preferred_role` claim is set, use it\.
+ If the `cognito:preferred_role` claim is not set, the `cognito:roles` claim is set, and `CustomRoleArn` is not specified in the call to `GetCredentialsForIdentity`, then the **Role resolution** setting in the console or the `AmbiguousRoleResolution` field \(in the `RoleMappings` parameter of the [SetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_SetIdentityPoolRoles.html) API\) is used to determine the role to be assigned\.

## Using rule\-based mapping to assign roles to users<a name="using-rules-to-assign-roles-to-users"></a>

Rules allow you to map claims from an identity provider token to IAM roles\.

Each rule specifies a token claim \(such as a user attribute in the ID token from an Amazon Cognito user pool\), match type, a value, and an IAM role\. The match type can be `Equals`, `NotEqual`, `StartsWith`, or `Contains`\. If a user has a matching value for the claim, the user can assume that role when the user gets credentials\. For example, you can create a rule that assigns a specific IAM role for users with a `custom:dept` custom attribute value of `Sales`\. 

**Note**  
In the rule settings, custom attributes require the `custom:` prefix to distinguish them from standard attributes\.

Rules are evaluated in order, and the IAM role for the first matching rule is used, unless `CustomRoleArn` is specified to override the order\. For more information about user attributes in Amazon Cognito user pools, see [User pool attributes](user-pool-settings-attributes.md)\.

You can set multiple rules for an authentication provider in the identity pool \(federated identities\) console\. Rules are applied in order\. You can drag the rules to change their order\. The first matching rule takes precedence\. If the match type is `NotEqual` and the claim doesn't exist, the rule is not evaluated\. If no rules match, the **Role resolution** setting is applied to either **Use default Authenticated role** or **DENY**\.

In the API and CLI, you can specify the role to be assigned when no rules match in the `AmbiguousRoleResolution` field of the [RoleMapping](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_RoleMapping.html) type, which is specified in the `RoleMappings` parameter of the [SetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_SetIdentityPoolRoles.html) API\.

For each user pool or other authentication provider configured for an identity pool, you can create up to 25 rules\. If you need more than 25 rules for a provider, please open a [Service Limit Increase](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) support case\.

## Token claims to use in rule\-based mapping<a name="token-claims-for-role-based-access-control"></a>

**Amazon Cognito**

An Amazon Cognito ID token is represented as a JSON Web Token \(JWT\)\. The token contains claims about the identity of the authenticated user, such as `name`, `family_name`, and `phone_number`\. For more information about standard claims, see the [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\. Apart from standard claims, the following are the additional claims specific to Amazon Cognito:
+ `cognito:groups`
+ `cognito:roles`
+ `cognito:preferred_role`

**Amazon**

The following claims, along with possible values for those claims, can be used with Login with Amazon:
+ `iss`: www\.amazon\.com
+ `aud`: App Id
+ `sub`: `sub` from the Login with Amazon token

**Facebook**

The following claims, along with possible values for those claims, can be used with Facebook:
+ `iss`: graph\.facebook\.com
+ `aud`: App Id
+ `sub`: `sub` from the Facebook token

**Google**

A Google token contains standard claims from the [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\. All of the claims in the OpenID token are available for rule\-based mapping\. See Google's [OpenID Connect](https://developers.google.com/identity/protocols/OpenIDConnect) site to learn about the claims available from the Google token\.

**Apple**

An Apple token contains standard claims from the [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\. See [Authenticating Users with Sign in with Apple](https://developer.apple.com/documentation/signinwithapplerestapi/authenticating_users_with_sign_in_with_apple) in Apple’s documentation to learn more about the claim available from the Apple token\. Apple's token doesn’t always contain `email`\.

**OpenID**

All of the claims in the Open Id token are available for rule\-based mapping\. For more information about standard claims, see the [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\. Refer to your OpenID provider documentation to learn about any additional claims that are available\.

**SAML**

Claims are parsed from the received SAML assertion\. All the claims that are available in the SAML assertion can be used in rule\-based mapping\.

## Best practices for role\-based access control<a name="best-practices-for-role-based-access-control"></a>

**Important**  
If the claim that you are mapping to a role can be modified by the end user, any end user can assume your role and set the policy accordingly\. Only map claims that cannot be directly set by the end user to roles with elevated permissions\. In an Amazon Cognito user pool, you can set per\-app read and write permissions for each user attribute\.

**Important**  
If you set roles for groups in an Amazon Cognito user pool, those roles are passed through the user's ID token\. To use these roles, you must also set **Choose role from token** for the authenticated role selection for the identity pool\.  
You can use the **Role resolution** setting in the console and the `RoleMappings` parameter of the [SetIdentityPoolRoles](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/API_SetIdentityPoolRoles.html) API to specify what the default behavior is when the correct role cannot be determined from the token\.