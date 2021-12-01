# Role trust and permissions<a name="role-trust-and-permissions"></a>

The way these roles differ is in their trust relationships\. Let's take a look at an example trust policy for an unauthenticated role:

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

This policy defines that we want to allow federated users from `cognito-identity.amazonaws.com` \(the issuer of the OpenID Connect token\) to assume this role\. Additionally, we make the restriction that the aud of the token, in our case the identity pool ID, matches our identity pool\. Finally, we specify that the amr of the token contains the value unauthenticated\.

When Amazon Cognito creates a token, it will set the `amr` of the token to be either "unauthenticated" or "authenticated" and in the authenticated case will include any providers used during authentication\. This means you can create a role that trusts only users that logged in via Facebook, simply by changing the amr clause to look like the following:

```
"ForAnyValue:StringLike": {
  "cognito-identity.amazonaws.com:amr": "graph.facebook.com"
}
```

Be careful when changing your trust relationships on your roles, or when trying to use roles across identity pools\. If your role is not configured to correctly trust your identity pool, you will see an exception from STS like the following:

```
AccessDenied -- Not authorized to perform sts:AssumeRoleWithWebIdentity
```

If you see this, double check that you are using an appropriate role for your identity pool and authentication type\.