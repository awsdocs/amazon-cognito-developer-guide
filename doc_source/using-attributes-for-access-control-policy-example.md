# Using attributes for access control policy example<a name="using-attributes-for-access-control-policy-example"></a>

Consider a scenario where an employee from the legal department of a company needs to list all files in buckets that belong to their department and are classified with their security level\. Assume the token that this employee gets from the identity provider contains the following claims\. 

**Claims**

```
            { .
              .
            "sub" : "57e7b692-4f66-480d-98b8-45a6729b4c88",
            "department" : "legal",
            "clearance" : "confidential",
             .
             .
            }
```

These attributes can be mapped to tags and referenced in IAM permissions policies as principal tags\. You can now manage access by changing the user profile on the identity provider's end\. Alternatively, you can change attributes on the resource side by using names or tags without changing the policy itself\.

The following permissions policy does two things:
+ Allows list access to all s3 buckets that end with a prefix that matches the user’s department name\.
+ Allows read access on files in these buckets as long as the clearance tag on the file matches user’s clearance attribute\.

**Permissions policy**

```
   {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:List*",
            "Resource": "arn:aws:s3:::*-${aws:PrincipalTag/department}"
        },
        {
            "Effect": "Allow",
            "Action": "s3:GetObject*",
            "Resource": "arn:aws:s3:::*-${aws:PrincipalTag/department}/*",
            "Condition": {
                "StringEquals": {
                    "s3:ExistingObjectTag/clearance": "${aws:PrincipalTag/clearance}"
                }
            }
        }
    ]
}
```

The trust policy determines who can assume this role\. The trust relationship policy allows the use of `sts:AssumeRoleWithWebIdentity` and `sts:TagSession` to allow access\. It adds conditions to restrict the policy to the identity pool that you created and it ensures that it’s for an authenticated role\.

**Trust policy**

```
            
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "cognito-identity.amazonaws.com"
      },
      "Action": [
        "sts:AssumeRoleWithWebIdentity",
        "sts:TagSession"
      ],
      "Condition": {
        "StringEquals": {
          "cognito-identity.amazonaws.com:aud": "IDENTITY-POOL-ID"
        },
        "ForAnyValue:StringLike": {
          "cognito-identity.amazonaws.com:amr": "authenticated"
        }
      }
    }
  ]
}
```