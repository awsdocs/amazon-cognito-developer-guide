# Custom sender Lambda triggers<a name="user-pool-lambda-custom-sender-triggers"></a>

Amazon Cognito user pools provide the Lambda triggers `CustomEmailSender` and `CustomSMSSender` to activate third\-party email and SMS notifications\. You can choose SMS and email providers to send notifications to users from within your Lambda function code\. When Amazon Cognito must send notifications like confirmation codes, verification codes, or temporary passwords to users, the events activate your configured Lambda functions\. Amazon Cognito sends the code and temporary passwords \(secrets\) to your activated Lambda functions\. Amazon Cognito encrypts these secrets with an AWS KMS customer managed key and the AWS Encryption SDK\. The AWS Encryption SDK is a client\-side encryption library that helps you to encrypt and decrypt generic data\.

**Note**  
To configure your user pools to use these Lambda triggers, you can use the AWS CLI or SDK\. These configurations aren't available from Amazon Cognito console\.

**[CustomEmailSender](user-pool-lambda-custom-email-sender.md)**  
Amazon Cognito invokes this trigger to send email notifications to users\. 

**[CustomSMSSender](user-pool-lambda-custom-sms-sender.md)**  
Amazon Cognito invokes this trigger to send SMS notifications to users\.

## Resources<a name="ser-pool-lambda-custom-sender-triggers-resources"></a>

The following resources can help you to use the `CustomEmailSender` and `CustomSMSSender` triggers\.

**AWS KMS**  
AWS KMS is a managed service to create and control AWS KMS keys\. These keys encrypt your data\. For more information see, [What is AWS Key Management Service?](/kms/latest/developerguide/overview.html)\.

**KMS key**  
A KMS key is a logical representation of a cryptographic key\. The KMS key includes metadata, such as the key ID, creation date, description, and key state\. The KMS key also contains the key material used to encrypt and decrypt data\. For more information see, [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#kms_keys)\.

**Symmetric KMS key**  
A symmetric KMS key is a 256\-bit encryption key that doesn't exit AWS KMS unencrypted\. To use a symmetric KMS key, you must call AWS KMS\. Amazon Cognito uses symmetric keys\. The same key encrypts and decrypts\. For more information see, [Symmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#symmetric-cmks)\. 