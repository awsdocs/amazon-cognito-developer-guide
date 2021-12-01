# Authorizing Amazon Cognito to send Amazon SES email on your behalf \(from a custom FROM email address\)<a name="cognito-user-pool-settings-ses-authorization-to-send-email"></a>

You can configure Amazon Cognito to send email from a custom FROM email address instead of its default address\. To use a custom address, you must give Amazon Cognito permission to send email message from an Amazon SES verified identity\. In most cases, you can grant permission by creating a sending authorization policy\. For more information, see [Using sending authorization with Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization.html) in the *Amazon Simple Email Service Developer Guide*\. 

When you configure a user pool to use Amazon SES for email messages, Amazon Cognito creates the `AWSServiceRoleForAmazonCognitoIdpEmailService` role in your account to grant access to Amazon SES\. No sending authorization policy is needed when the `AWSServiceRoleForAmazonCognitoIdpEmailService` service\-linked role is used\. You only need to add a sending authorization policy when you use both the default email functionality in your user pool *and* a verified Amazon SES identity as the FROM address\.

For more information about the service\-linked role that Amazon Cognito creates, see [Using service\-linked roles for Amazon Cognito](using-service-linked-roles.md)\.

The following example sending authorization policy grants Amazon Cognito a limited ability to use an Amazon SES verified identity\. Amazon Cognito can only send email messages when it does so on behalf of both the user pool in the `aws:SourceArn` condition and the account in the `aws:SourceAccount` condition\. For more examples, see [Amazon SES sending authorization policy examples](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policy-examples.html) in the *Amazon Simple Email Service Developer Guide*\.

**Note**  
In this example, the "Sid" value is an arbitrary string that uniquely identifies the statement\. For more information about policy syntax, see [Amazon SES sending authorization policies](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policies.html) in the *Amazon Simple Email Service Developer Guide*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "stmnt1234567891234",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "email.cognito-idp.amazonaws.com"
                ]
            },
            "Action": [
                "SES:SendEmail",
                "SES:SendRawEmail"
            ],
            "Resource": "<your SES identity ARN>",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceAccount": "<your account number>"
                },
                "ArnLike": {
                    "AWS:SourceArn": "<your identity pool ARN>"
                }
            }
        }
    ]
}
```

The Amazon Cognito console adds a similar policy for you when you select an Amazon SES identity from the drop\-down menu\. If you use the CLI or API to configure the user pool, you must attach a policy structured like the previous example to your Amazon SES Identity\.