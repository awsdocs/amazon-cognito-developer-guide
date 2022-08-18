# Role trust and permissions<a name="role-trust-and-permissions"></a>

The way these roles differ is in their trust relationships\. The following is an example trust policy for an unauthenticated role:

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
          "cognito-identity.amazonaws.com:amr": "unauthenticated"
        }
      }
    }
  ]
}
```

This policy grants federated users from `cognito-identity.amazonaws.com` \(the issuer of the OpenID Connect token\) permission to assume this role\. Additionally, the policy restricts the `aud` of the token, in this case the identity pool ID, to match the identity pool\. Finally, the policy specifies that one of the array members of the multi\-value `amr` claim of the token issued by the Amazon Cognito `GetOpenIdToken` API operation has the value `unauthenticated`\.

When Amazon Cognito creates a token, it sets the `amr` of the token as either `unauthenticated` or `authenticated`\. If `amr` is `authenticated`, the token includes any providers used during authentication\. This means that you can create a role that trusts only users that logged in via Facebook by changing the `amr` condition as shown:

```
"ForAnyValue:StringLike": {
  "cognito-identity.amazonaws.com:amr": "graph.facebook.com"
}
```

Be careful when changing your trust relationships on your roles, or when trying to use roles across identity pools\. If you don't configure your role correctly to trust your identity pool, an exception from STS results, like the following:

```
AccessDenied -- Not authorized to perform sts:AssumeRoleWithWebIdentity
```

If you see this message, check that your identity pool and authentication type have an appropriate role\.

You may also experience this error when calling AWSCognitoIdentityService.GetCredentialsForIdentity:

```
{"__type":"InvalidIdentityPoolConfigurationException","message":"Invalid identity pool configuration. Check assigned IAM roles for this pool."}
```

If you see this message, check that you've allowed `sts:AssumeRoleWithWebIdentity` correctly rather than just `sts:AssumeRole` in your trust relationship policy\.
