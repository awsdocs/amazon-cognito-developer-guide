# Custom SMS sender Lambda trigger<a name="user-pool-lambda-custom-sms-sender"></a>

Amazon Cognito invokes the custom SMS sender trigger so that a third\-party provider can send SMS notifications to your users from your AWS Lambda function code\. Amazon Cognito sends SMS message events as requests to a Lambda function\. The custom code of your function must then process and deliver the message\.

**Note**  
Currently, you can't assign a custom SMS sender trigger in the Amazon Cognito console\. You can assign a trigger with the `LambdaConfig` parameter in a `CreateUserPool` or `UpdateUserPool` API request\.

To set up this trigger, perform the following steps:

1. Create an encryption key in AWS Key Management Service \(AWS KMS\)\. Amazon Cognito generates secrets \(temporary passwords and authorization codes\), then uses this KMS key to encrypt the secrets\. You can then use the AWS Encryption SDK in your Lambda function to decrypt the codes and send them to the user in plaintext\.

1. Create a Lambda function that you want to assign as your custom SMS sender trigger\. Grant the execution role that you assign to your Lambda function `kms:Decrypt` permissions for your KMS key\.

1. Grant Amazon Cognito service principal `cognito-idp.amazonaws.com` access to invoke the Lambda function and to create a grant for your KMS key\.

1. Write Lambda function code that directs your SMS messages to custom delivery methods or third\-party providers\.

1. Update the user pool so that it uses a custom sender Lambda trigger\.

**Important**  
Create a new symmetric AWS KMS key when you configure a custom email or custom SMS sender function\. Amazon Cognito uses your configured KMS key to encrypt codes or temporary passwords\. Amazon Cognito sends the base64\-encoded ciphertext to your Lambda functions\. For more information, see [Symmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#symmetric-cmks)\.

## Custom SMS sender Lambda trigger parameters<a name="custom-sms-sender-parameters"></a>

These are the parameters that Amazon Cognito passes to this Lambda function along with the event information in the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-syntax-shared)\.

------
#### [ JSON ]

```
{
    "request": {
        "type": "customSMSSenderRequestV1",
        "code": "string",
        "clientMetadata": {
            "string": "string",
             . . .
            },
        "userAttributes": {
            "string": "string",
            . . .
         }
}
```

------

### Custom SMS sender request parameters<a name="custom-sms-sender-request-parameters"></a>

**type**  
The request version\. For a custom SMS sender event, the value of this string is always `customSMSSenderRequestV1`\.

**code**  
The encrypted code that your function can decrypt and send to your user\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the custom SMS sender Lambda function trigger\. To pass this data to your Lambda function, you can use the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API actions\. Amazon Cognito doesn't include data from the ClientMetadata parameter in [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) and [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API operations in the request that it passes to the post authentication function\.

**userAttributes**  
One or more key\-value pairs that represent user attributes\.

### Custom SMS sender response parameters<a name="custom-sms-sender-response-parameters"></a>

Amazon Cognito doesn't expect any additional return information in the response\. Your function can use API operations to query and modify your resources, or record event metadata to an external system\.

## Activating the custom SMS sender Lambda trigger<a name="enable-custom-sms-sender-lambda-trigger"></a>

You can set up a custom SMS sender trigger that uses custom logic to send SMS messages for your user pool\. The following procedure assigns a custom SMS trigger, a custom email trigger, or both to your user pool\. After you add your custom SMS sender trigger, Amazon Cognito always sends user attributes, including the phone number, and the one\-time code to your Lambda function instead of the default behavior that sends an SMS message with Amazon Simple Notification Service\.

**Important**  
Amazon Cognito HTML\-escapes reserved characters like `<` \(`&lt;`\) and `>` \(`&gt;`\) in your user's temporary password\. These characters might appear in temporary passwords that Amazon Cognito sends to your custom email sender function, but don't appear in temporary verification codes\. To send temporary passwords, your Lambda function must unescape these characters after it decrypts the password, and before it sends the message to your user\.

1. Create an encryption key in AWS KMS\. This key encrypts temporary passwords and authorization codes that Amazon Cognito generates\. You can then decrypt these secrets in the custom sender Lambda function and send them to your user in plaintext\.

1. Grant Amazon Cognito service principal `cognito-idp.amazonaws.com` access to encrypt codes with the KMS key\.

   Apply the following resource\-based policy to your KMS key\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [{
           "Effect": "Allow",
           "Principal": {
               "Service": "cognito-idp.amazonaws.com"
           },
           "Action": "kms:CreateGrant",
           "Resource": "arn:aws:kms:us-west-2:111222333444:key/1example-2222-3333-4444-999example",
           "Condition": {
               "StringEquals": {
                   "aws:SourceAccount": "111222333444"
               },
               "ArnLike": {
                   "aws:SourceArn": "arn:aws:cognito-idp:us-west-2:111222333444:userpool/us-east-1_EXAMPLE"
               }
           }
       }]
   }
   ```

1. Create a Lambda function for the custom sender trigger\. Amazon Cognito uses the [AWS encryption SDK](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction.html) to encrypt the secrets, temporary passwords and codes that authorize your users' API requests\.

   1. Assign an IAM role to your Lambda function that has, at minimum, `kms:Decrypt` permissions for your KMS key\.

1. Grant Amazon Cognito service principal `cognito-idp.amazonaws.com` access to invoke the Lambda function\.

   The following AWS CLI command grants Amazon Cognito permission to invoke your Lambda function:

   ```
           aws lambda add-permission --function-name lambda_arn --statement-id "CognitoLambdaInvokeAccess" --action lambda:InvokeFunction --principal cognito-idp.amazonaws.com
   ```

1. Compose your Lambda function code to send your messages\. Amazon Cognito uses AWS Encryption SDK to encrypt secrets before Amazon Cognito sends the secrets to the custom sender Lambda function\. In your function, decrypt the secret and process any relevant metadata\. Then send the code, your own custom message, and destination phone number to the custom API that delivers your message\.

1. Add the AWS Encryption SDK to your Lambda function\. For more information, see [AWS Encryption SDK programming languages](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/programming-languages.html)\. To update the Lambda package, complete the following steps\.

   1. Export your Lambda function as a \.zip file in the AWS Management Console\.

   1. Open your function and add the AWS Encryption SDK\. For more information and download links, see [AWS Encryption SDK programming languages](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/programming-languages.html) in the *AWS Encryption SDK Developer Guide*\.

   1. Zip your function with your SDK dependencies, and upload the function to Lambda\. For more information, see [Deploying Lambda functions as \.zip file archives](https://docs.aws.amazon.com/lambda/latest/dg/configuration-function-zip.html#configuration-function-create) in the *AWS Lambda Developer Guide*\.

1. Update your user pool to add custom sender Lambda triggers\. Include a `CustomSMSSender` or `CustomEmailSender` parameter in an `UpdateUserPool` API request\. The `UpdateUserPool` API operation requires all the parameters of your user pool *and* the parameters that you want to change\. If you don't provide all relevant parameters, Amazon Cognito sets the values of any missing parameters to their defaults\. As demonstrated in the example that follows, include entries for all Lambda functions that you want to add to or keep in your user pool\. For more information, see [Updating user pool configuration](cognito-user-pool-updating.md)\.

   ```
       #Send this parameter in an 'aws cognito-idp update-user-pool' CLI command, including any existing 
       #user pool configurations.
                 
         --lambda-config "PreSignUp={LambdaVersion=V1_0,LambdaArn=lambda-arn}, \
                          CustomSMSSender={LambdaVersion=V1_0,LambdaArn=lambda-arn}, \
                          CustomEmailSender={LambdaVersion=V1_0,LambdaArn=lambda-arn}, \
                          KMSKeyID=key-id"
   ```

To remove a custom sender Lambda trigger with an `update-user-pool` AWS CLI, omit the `CustomSMSSender` or `CustomEmailSender` parameter from `--lambda-config`, and include all other triggers that you want to use with your user pool\.

To remove a custom sender Lambda trigger with an `UpdateUserPool` API request, omit the `CustomSMSSender` or `CustomEmailSender` parameter from the request body that contains the rest of your user pool configuration\.

## Code example<a name="custom-sms-sender-code-examples"></a>

The following Node\.js example processes an SMS message event in your custom SMS sender Lambda function\. This example assumes your function has two environment variables defined\.

**`KEY_ALIAS`**  
The [alias](https://docs.aws.amazon.com/kms/latest/developerguide/kms-alias.html) of the KMS key that you want to use to encrypt and decrypt your users' codes\.

**`KEY_ARN`**  
The Amazon Resource Name \(ARN\) of the KMS key that you want to use to encrypt and decrypt your users' codes\.

```
const AWS = require('aws-sdk');
const b64 = require('base64-js');
const encryptionSdk = require('@aws-crypto/client-node');
//Configure the encryption SDK client with the KMS key from the environment variables.  
const { encrypt, decrypt } = encryptionSdk.buildClient(encryptionSdk.CommitmentPolicy.REQUIRE_ENCRYPT_ALLOW_DECRYPT);
const generatorKeyId = process.env.KEY_ALIAS;
const keyIds = [ process.env.KEY_ARN ];
const keyring = new encryptionSdk.KmsKeyringNode({ generatorKeyId, keyIds })
exports.handler = async (event) => {
	//Decrypt the secret code using encryption SDK.
	let plainTextCode;
	if(event.request.code){
		const { plaintext, messageHeader } = await decrypt(keyring, b64.toByteArray(event.request.code));
		plainTextCode = plaintext
	}
	//PlainTextCode now contains the decrypted secret.
	if(event.triggerSource == 'CustomSMSSender_SignUp'){
		//Send an SMS message to your user via a custom provider.
		//Include the temporary password in the message.
	}
	else if(event.triggerSource == 'CustomSMSSender_ResendCode'){
	}
	else if(event.triggerSource == 'CustomSMSSender_ForgotPassword'){
	}
	else if(event.triggerSource == 'CustomSMSSender_UpdateUserAttribute'){
	}
	else if(event.triggerSource == 'CustomSMSSender_VerifyUserAttribute'){
	}
	else if(event.triggerSource == 'CustomSMSSender_AdminCreateUser'){
	}
	else if(event.triggerSource == 'CustomSMSSender_AccountTakeOverNotification'){
	}
	return;
};
```

**Topics**
+ [Custom SMS sender Lambda trigger parameters](#custom-sms-sender-parameters)
+ [Activating the custom SMS sender Lambda trigger](#enable-custom-sms-sender-lambda-trigger)
+ [Code example](#custom-sms-sender-code-examples)
+ [Evaluate SMS message capabilities with a custom SMS sender function](#sms-to-email-example)
+ [Custom SMS sender Lambda trigger sources](#trigger-source)

## Evaluate SMS message capabilities with a custom SMS sender function<a name="sms-to-email-example"></a>

A custom SMS sender Lambda function accepts the SMS messages that your user pool would send, and the function delivers the content based on your custom logic\. Amazon Cognito sends the [Custom SMS sender Lambda trigger parameters](#custom-sms-sender-parameters) to your function\. Your function can do what you want with this information\. For example, you can send the code to an Amazon Simple Notification Service \(Amazon SNS\) topic\. An Amazon SNS topic subscriber can be an SMS message, an HTTPS endpoint, or an email address\.

To create a test environment for Amazon Cognito SMS messaging with a custom SMS sender Lambda function, see [amazon\-cognito\-user\-pool\-development\-and\-testing\-with\-sms\-redirected\-to\-email](https://github.com/aws-samples/amazon-cognito-user-pool-development-and-testing-with-sms-redirected-to-email) in the [aws\-samples library on GitHub](https://github.com/aws-samples)\. The repository contains AWS CloudFormation templates that can create a new user pool, or work with a user pool that you already have\. These templates create Lambda functions and an Amazon SNS topic\. The Lambda function that the template assigns as a custom SMS sender trigger, redirects your SMS messages to the subscribers to the Amazon SNS topic\. 

When you deploy this solution to a user pool, all messages that Amazon Cognito usually sends through SMS messaging, the Lambda function instead sends to a central email address\. Use this solution to customize and preview SMS messages, and to test the user pool events that cause Amazon Cognito to send an SMS message\. After you complete your tests, roll back the CloudFormation stack, or remove the custom SMS sender function assignment from your user pool\.

**Important**  
Don't use the templates in [amazon\-cognito\-user\-pool\-development\-and\-testing\-with\-sms\-redirected\-to\-email](https://github.com/aws-samples/amazon-cognito-user-pool-development-and-testing-with-sms-redirected-to-email) to build a production environment\. The custom SMS sender Lambda function in the solution *simulates* SMS messages, but the Lambda function sends them all to a single central email address\. Before you can send SMS messages in a production Amazon Cognito user pool, you must complete the requirements shown at [SMS message settings for Amazon Cognito user pools](user-pool-sms-settings.md)\.

## Custom SMS sender Lambda trigger sources<a name="trigger-source"></a>

The following table shows the triggering event for custom SMS trigger sources in your Lambda code\.


| `TriggerSource value` | Event | 
| --- | --- | 
| CustomSMSSender\_SignUp | A user signs up and Amazon Cognito sends a welcome message\. | 
| CustomSMSSender\_ForgotPassword | A user requests a code to reset their password\. | 
| CustomSMSSender\_ResendCode | A user requests a replacement code to reset their password\. | 
| CustomSMSSender\_VerifyUserAttribute | A user creates a new email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomSMSSender\_UpdateUserAttribute | A user updates an email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomSMSSender\_Authentication | A user configured with SMS multi\-factor authentication \(MFA\) signs in\. | 
| CustomSMSSender\_AdminCreateUser | You create a new user in your user pool and Amazon Cognito sends them a temporary password\. | 