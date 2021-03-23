# Custom sender Lambda Triggers<a name="user-pool-lambda-custom-sender-triggers"></a>

Amazon Cognito user pools provide two Lambda triggers `CustomEmailSender` and `CustomSMSSender` to enable third\-party email and SMS notifications\. You can Use your choice of SMS and email providers to send notifications to users from within your Lambda function code\. These trigger your configured Lambda functions when notifications like confirmation codes, verification codes, or temporary passwords need to be sent to users\. Amazon Cognito sends the code and temporary passwords \(secrets\) to your triggered Lambda functions\. The secrets will be encrypted using a AWS KMS customer managed key and the AWS Encryption SDK\. The The AWS Encryption SDK is a client\-side encryption library that helps you to encrypt and decrypt generic data\.

**Note**  
You can use the AWS CLI or SDK to configure your user pools to use these Lambda triggers\. These configurations are not available from Amazon Cognito console\.

**[CustomEmailSender](user-pool-lambda-custom-email-sender.md)**  
Amazon Cognito invokes this trigger to send email notifications to users\. 

**[CustomSMSSender](user-pool-lambda-custom-sms-sender.md)**  
Amazon Cognito invokes this trigger to send SMS notifications to users\.

## Resources<a name="ser-pool-lambda-custom-sender-triggers-resources"></a>

The following resources can assist you with using the `CustomEmailSender` and `CustomSMSSender` triggers\.

**AWS KMS**  
AWS KMS is a managed service that makes it easy for you to create and control customer master keys \(CMKs\), which are the encryption keys that are used to encrypt your data\. For more information see, [What is AWS Key Management Service?](/kms/latest/developerguide/overview.html)\.

**Customer master key**  
A customer master key \(CMK\) is a logical representation of a master key\. The CMK includes metadata, such as the key ID, creation date, description, and key state\. The CMK also contains the key material used to encrypt and decrypt data\. For more information see, [Customer master keys](/kms/latest/developerguide/concepts.html#master_keys)\.

**Symmetric customer master key**  
A symmetric customer master key represents a 256\-bit encryption key that never leaves AWS KMS unencrypted\. To use a symmetric CMK, you must call AWS KMS\. Symmetric keys are used in symmetric encryption, where the same key is used for encryption and decryption\. For more information see, [Symmetric customer master keys](/kms/latest/developerguide/symm-asymm-concepts.html#symmetric-cmks)\. 