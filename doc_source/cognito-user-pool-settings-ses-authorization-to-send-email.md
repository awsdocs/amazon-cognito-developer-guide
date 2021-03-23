# Authorizing Amazon Cognito to Send Amazon SES Email on Your Behalf \(from a Custom FROM Email Address\)<a name="cognito-user-pool-settings-ses-authorization-to-send-email"></a>

If you want to send email from a custom FROM email address instead of the default, Amazon Cognito needs your permission to send email messages to your users on behalf of your Amazon SES verified identity\. To grant that permission, create a sending authorization policy\. For more information, see [Using Sending Authorization with Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization.html) in the *Amazon Simple Email Service Developer Guide*\. 

The following is an example of an Amazon SES sending authorization policy for Amazon Cognito user pools\. For more examples, see [Amazon SES Sending Authorization Policy Examples](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policy-examples.html) in the *Amazon Simple Email Service Developer Guide*\.

**Note**  
In this example, the "Sid" value is an arbitrary string that uniquely identifies the statement\. For more information about policy syntax, see [Amazon SES Sending Authorization Policies](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/sending-authorization-policies.html) in the *Amazon Simple Email Service Developer Guide*\.

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "stmnt1234567891234",
            "Effect": "Allow",
            "Principal": {
                "Service": "cognito-idp.amazonaws.com"
            },
            "Action": [
                "ses:SendEmail",
                "ses:SendRawEmail"
            ],
            "Resource": "<your SES identity ARN>"
        }
    ]
}
```

The Amazon Cognito console adds this policy for you when you select an Amazon SES identity from the drop\-down menu\. If you use the CLI or API to configure the user pool, you must attach this policy to your Amazon SES Identity\.