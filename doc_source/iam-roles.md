# IAM Roles<a name="iam-roles"></a>

In the process of creating an identity pool, you'll be prompted to update the IAM roles that your users assume\. IAM roles work like this: When a user logs in to your app, Amazon Cognito generates temporary AWS credentials for the user\. These temporary credentials are associated with a specific IAM role\. The IAM role lets you define a set of permissions to access your AWS resources\.

You can specify default IAM roles for authenticated and unauthenticated users\. In addition, you can define rules to choose the role for each user based on claims in the user's ID token\. For more information, see [Role\-Based Access Control](role-based-access-control.md)\.

By default, the Amazon Cognito Console creates IAM roles that provide access to Amazon Mobile Analytics and to Amazon Cognito Sync\. Alternatively, you can choose to use existing IAM roles\.

To modify IAM roles, thereby allowing or restricting access to other services, [log in to the IAM Console](https://console.aws.amazon.com/iam/home)\. Then click Roles and select a role\. The policies attached to the selected role are listed in the Permissions tab\. You can customize an access policy by clicking the corresponding Manage Policy link\. To learn more about using and defining policies, see [Overview of IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PoliciesOverview.html)\. 

**Note**  
As a best practice, define policies that follow the principle of granting *least privilege*\. In other words, the policies include only the permissions that users require to perform their tasks\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.  
Remember that unauthenticated identities are assumed by users who do not log in to your app\. Typically, the permissions that you assign for unauthenticated identities should be more restrictive than those for authenticated identities\.

**Topics**
+ [Set Up a Trust Policy](#trust-policies)
+ [Access Policies](#access-policies)

## Set Up a Trust Policy<a name="trust-policies"></a>

Amazon Cognito leverages IAM roles to generate temporary credentials for your application's users\. Access to permissions is controlled by a role's trust relationships\. Learn more about [Role Trust and Permissions](role-trust-and-permissions.md)\.

**Reuse Roles Across Identity Pools**

To reuse a role across multiple identity pools, because they share a common permission set, you can include multiple identity pools, like this:

```
"StringEquals": {
    "cognito-identity.amazonaws.com:aud": [
        "us-east-1:12345678-abcd-abcd-abcd-123456790ab",
        "us-east-1:98765432-dcba-dcba-dcba-123456790ab"
    ]
}
```

**Limit Access to Specific Identities**

To create a policy limited to a specific set of app users, check the value of `cognito-identity.amazonaws.com:sub`:

```
"StringEquals": {
    "cognito-identity.amazonaws.com:aud": "us-east-1:12345678-abcd-abcd-abcd-123456790ab",
    "cognito-identity.amazonaws.com:sub": [
        "us-east-1:12345678-1234-1234-1234-123456790ab",
        "us-east-1:98765432-1234-1234-1243-123456790ab"
    ]
}
```

**Limit Access to Specific Providers**

To create a policy limited to users who have logged in with a specific provider \(perhaps your own login provider\), check the value of `cognito-identity.amazonaws.com:amr`:

```
"ForAnyValue:StringLike": {
    "cognito-identity.amazonaws.com:amr": "login.myprovider.myapp"
}
```

For example, an app that trusts only Facebook would have the following amr clause:

```
"ForAnyValue:StringLike": {
    "cognito-identity.amazonaws.com:amr": "graph.facebook.com"
}
```

## Access Policies<a name="access-policies"></a>

The permissions attached to a role are effective across all users that assume that role\. If you want to partition your users' access, you can do so via policy variables\. Be careful when including your users' identity IDs in your access policies, particularly for unauthenticated identities as these may change if the user chooses to login\.

For additional security protection, Amazon Cognito applies a scope\-down policy to credentials vended by `GetCredentialForIdentity` to prevent access to services other than the ones listed below for your unauthenticated users\. In other words, this policy allows an identity using these credentials with access to only the following services: 


|  |  |  |  |  |  |  | 
| --- |--- |--- |--- |--- |--- |--- |
|  AWS AppSync  |  Amazon Comprehend  | GameLift |  Amazon Lex  |  Amazon Polly  |  Amazon SimpleDB  |  Amazon Transcribe  | 
|  CloudWatch  |  DynamoDB  | AWS IoT |  Amazon Machine Learning  |  Amazon Rekognition  |  Amazon SES  | Amazon Translate | 
|  Amazon Cognito Identity  |  Amazon Kinesis Data Firehose  | AWS KMS | Amazon Mobile Analytics |  Amazon Sumerian  |  Amazon SNS  |  | 
|  Amazon Cognito Sync  |  Amazon Kinesis Data Streams  | AWS Lambda |  Amazon Pinpoint  |  Amazon S3  |  Amazon SQS  |  | 

If you need access to something other than these services for your unauthenticated users, you must use the basic authentication flow\. If you are getting `NotAuthorizedException` and you have enabled access to the service in your unauthenticated role policy, this is likely the reason\.

### Access Policy Examples<a name="access-policy-examples"></a>

In this section, you can find example Amazon Cognito access policies that grant only the permissions your identities need to complete a specific operation\. You can further limit the permissions for a given identity ID by using policy variables where possible\. For example, using $\{cognito\-identity\.amazonaws\.com:sub\}\. For more information, see [Understanding Amazon Cognito Authentication Part 3: Roles and Policies](https://aws.amazon.com/blogs/mobile/understanding-amazon-cognito-authentication-part-3-roles-and-policies/) on the *AWS Mobile Blog*\.

**Note**  
As a security best practice, policies should include only the permissions that users require to perform their tasks\. This means that you should try to always scope access to an individual identity for objects whenever possible\.

**Example 1: Allow an Identity to Have Read Access to a Single Object in S3**

The following access policy grants read permissions to an identity to retrieve a single object from a given S3 bucket\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": ["arn:aws:s3:::mybucket/assets/my_picture.jpg"]
    }
  ]
}
```

**Example 2: Allow an Identity to Have Both Read and Write Access to Identity Specific Paths in S3**

The following access policy grants read and write permissions to access a specific prefix "folder" in an S3 bucket by mapping the prefix to the `${cognito-identity.amazonaws.com:sub}` variable\.

With this policy, an identity such as `us-east-1:12345678-1234-1234-1234-123456790ab` inserted via `${cognito-identity.amazonaws.com:sub}` will be able to get, put and list objects into `arn:aws:s3:::mybucket/us-east-1:12345678-1234-1234-1234-123456790ab`\. However, the identity would not be granted access to other objects in `arn:aws:s3:::mybucket`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": ["s3:ListBucket"],
      "Effect": "Allow",
      "Resource": ["arn:aws:s3:::mybucket"],
      "Condition": {"StringLike": {"s3:prefix": ["${cognito-identity.amazonaws.com:sub}/*"]}}
    },
    {
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": ["arn:aws:s3:::mybucket/${cognito-identity.amazonaws.com:sub}/*"]
    }
  ]
}
```

**Example 3: Assign Identities Fine\-Grained Access to Amazon DynamoDB**

The following access policy provides fine\-grained access control to Amazon DynamoDB resources using Amazon Cognito variables that grant access to items in DynamoDB by identity ID\. For more information, see [Using IAM Policy Conditions for Fine\-Grained Access Control](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/specifying-conditions.html) in the *Amazon DynamoDB Developer Guide*\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:BatchGetItem",
        "dynamodb:Query",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteItem",
        "dynamodb:BatchWriteItem"
      ],
      "Resource": [
        "arn:aws:dynamodb:us-west-2:123456789012:table/MyTable"
      ],
      "Condition": {
        "ForAllValues:StringEquals": {
          "dynamodb:LeadingKeys":  ["${cognito-identity.amazonaws.com:sub}"]
        }
      }
    }
  ]
}
```

**Example 4: Allow an Identity to Execute an AWS Lambda Function Invocation**

The following access policy grants an identity permissions to execute an AWS Lambda function\.

```
{
    "Version": "2012-10-17",
    "Statement": [
         { 
             "Effect": "Allow",
             "Action": "lambda:InvokeFunction",
             "Resource": [
                 "arn:aws:lambda:us-west-2:123456789012:function:MyFunction"
             ]
         }
     ]
 }
```

**Example 5: Allow an Identity to Publish Records to an Amazon Kinesis Data Stream**

The following access policy allows an identity to use the `PutRecord` operation with any of the Kinesis Data Streams\. It can be applied to users that need to add data records to all streams in an account\. For more information, see [Controlling Access to Amazon Kinesis Data Streams Resources Using IAM](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html) in the *Amazon Kinesis Data Streams Developer Guide*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "kinesis:PutRecord",
            "Resource": [
                "arn:aws:kinesis:us-east-1:111122223333:stream/stream1"
            ]
        }
    ]
}
```

**Example 6: Allow an Identity Access to Their Data in the Amazon Cognito Sync store**

The following access policy grants an identity permissions to only their data in the Amazon Cognito Sync store\.

```
{
  "Version": "2012-10-17",
  "Statement":[{
      "Effect":"Allow",
      "Action":"cognito-sync:*",
      "Resource":["arn:aws:cognito-sync:us-east-1:123456789012:identitypool/${cognito-identity.amazonaws.com:aud}/identity/${cognito-identity.amazonaws.com:sub}/*"]
      }]
  }
```