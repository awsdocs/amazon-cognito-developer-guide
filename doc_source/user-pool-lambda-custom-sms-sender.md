# Custom SMS sender Lambda trigger<a name="user-pool-lambda-custom-sms-sender"></a>

Amazon Cognito invokes the custom SMS sender trigger so that a third\-party provider can send SMS notifications to your users from your AWS Lambda function code\. Amazon Cognito sends SMS message events as requests to a Lambda function\. The custom code of your function must then process and deliver the message\.

**Note**  
Currently, you can't assign a custom SMS sender trigger in the Amazon Cognito console\. You can assign a trigger with the `LambdaConfig` parameter in a `CreateUserPool` or `UpdateUserPool` API request\.

To set up this trigger, perform the following steps:

1. Create a Lambda function that you want to assign as your custom SMS sender trigger\.

1. Create an encryption key in AWS Key Management Service \(AWS KMS\)\. Amazon Cognito generates secrets \(temporary passwords and authorization codes\), then uses this key to encrypt the secrets\. You can then use the AWS Encryption SDK in your Lambda function to decrypt the codes and send them to the user in plaintext\.

1. Grant Amazon Cognito service principal `cognito-idp.amazonaws.com` access to invoke the Lambda function\.

1. Write Lambda function code that directs your SMS messages to custom delivery methods or third\-party providers\.

1. Update the user pool so that it uses a custom sender Lambda trigger\.

**Important**  
Create a new symmetric AWS KMS key when you configure a custom email or custom SMS sender function\. Amazon Cognito uses your configured KMS key to encrypt codes or temporary passwords\. Amazon Cognito sends the base64 encoded ciphertext to your Lambda functions\. For more information, see [Symmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#symmetric-cmks)\.

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

To set up a custom SMS sender trigger that uses custom logic to send SMS messages for your user pool, activate the trigger as follows\.

**Step 1: Create a Lambda function**  
 Create a Lambda function for the custom SMS sender trigger\. Amazon Cognito uses the [AWS encryption SDK](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction.html) to encrypt the secrets \(temporary passwords or authorization codes\)\.

**Step 2: Create an encryption key in AWS KMS**  
Create an encryption key in AWS KMS\. This key encrypts temporary passwords and authorization codes that Amazon Cognito generates\. You can then decrypt these secrets in the custom sender Lambda function and send them to the user in plaintext\.

**Step 3: Grant Amazon Cognito service principal cognito\-idp\.amazonaws\.com access to invoke the Lambda function**  
Use the following command to grant access to the Lambda function:

```
        aws lambda add-permission --function-name lambda_arn --statement-id "CognitoLambdaInvokeAccess" --action lambda:InvokeFunction --principal cognito-idp.amazonaws.com
```

**Step 4: Edit the code to use custom sender**  
Amazon Cognito uses AWS Encryption SDK to encrypt secrets \(temporary passwords and authorization codes\) before Amazon Cognito sends the secrets to the custom sender Lambda function\. Decrypt these secrets before you send them to users through the custom provider of your choice\. To use the AWS Encryption SDK with your Lambda function, you must package the SDK with your function\. For information, see [Installing the AWS encryption SDK for JavaScript](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/javascript-installation.html)\. To update the Lambda package, complete the following steps:

1. Export the Lambda function package from the console\.

1. Unzip the package\.

1. Add the AWS Encryption SDK to the package\. For example, if you are using Node\.js, then add the `node_modules` directory and include the libraries from @aws\-crypto/client\-node\.

1. Recreate the package\.

1. Update the Lambda function code from the modified directory\.

**Step 5: Update user pool to add custom sender Lambda triggers**  
Update the user pool with a `CustomSMSSender` parameter in an `UpdateUserPool` API operation\. `UpdateUserPool` requires all the parameters of your user pool as well as the parameters that you want to change\. If you don't provide all relevant parameters, Amazon Cognito sets the values of any missing parameters to their defaults\. For more information, see [Updating user pool configuration](cognito-user-pool-updating.md)\.

```
    #Send this parameter in an 'aws cognito-idp update-user-pool' CLI command, along with any existing user pool configurations.
              
      --lambda-config "CustomSMSSender={LambdaVersion=V1_0,LambdaArn= lambda-arn },KMSKeyID= key-id"
```

To remove a custom SMS sender Lambda trigger with the AWS CLI, omit the `CustomSMSSender` parameter from `--lambda-config` and include all other triggers that you want to use with your user pool\. To remove a custom SMS sender Lambda trigger with an `UpdateUserPool` API request, remove `CustomSMSSender` from the request body that contains the rest of your user pool configuration\. For more information, see [Updating a user pool with the Amazon Cognito API or AWS CLI](cognito-user-pool-updating.md#cognito-user-pool-updating-api-cli)\.

## Code examples<a name="code-examples"></a>

The following Node\.js example shows how to process an SMS message event in your custom SMS sender Lambda function\.

```
            const AWS = require('aws-sdk');
            const b64 = require('base64-js');
            const encryptionSdk = require('@aws-crypto/client-node');
            
            #Configure the encryption SDK client with the KMS key from the environment variables.  
            
            const { encrypt, decrypt } = encryptionSdk.buildClient(encryptionSdk.CommitmentPolicy.REQUIRE_ENCRYPT_ALLOW_DECRYPT);
            const generatorKeyId = process.env.KEY_ALIAS;
            const keyIds = [ process.env.KEY_ID ];
            const keyring = new encryptionSdk.KmsKeyringNode({ generatorKeyId, keyIds })
            exports.handler = async (event) => {

            #Decrypt the secret code using encryption SDK.
          
            let plainTextCode;
            if(event.request.code){
            const { plaintext, messageHeader } = await decrypt(keyring, b64.toByteArray(event.request.code));
            plainTextCode = plaintext
            }

            #PlainTextCode now has the decrypted secret.
            
            if(event.triggerSource == 'CustomSMSSender_SignUp'){
            
           #Send sms to end-user using custom or 3rd party provider.
           #Include temporary password in the email.
            
            }else if(event.triggerSource == 'CustomSMSSender_ResendCode'){
            
            }else if(event.triggerSource == 'CustomSMSSender_ForgotPassword'){
            
            }else if(event.triggerSource == 'CustomSMSSender_UpdateUserAttribute'){
            
            }else if(event.triggerSource == 'CustomSMSSender_VerifyUserAttribute'){
            
            }else if(event.triggerSource == 'CustomSMSSender_AdminCreateUser'){
            
            }else if(event.triggerSource == 'CustomSMSSender_AccountTakeOverNotification'){
            
            }
            
            return;
            };
```

**Topics**
+ [Custom SMS sender Lambda trigger parameters](#custom-sms-sender-parameters)
+ [Activating the custom SMS sender Lambda trigger](#enable-custom-sms-sender-lambda-trigger)
+ [Code examples](#code-examples)
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


| `TriggerSource value` | Triggering event | 
| --- | --- | 
| CustomSMSSender\_SignUp | A user signs up and Amazon Cognito sends a welcome message\. | 
| CustomSMSSender\_ForgotPassword | A user requests a code to reset their password\. | 
| CustomSMSSender\_ResendCode | A user requests a replacement code to reset their password\. | 
| CustomSMSSender\_VerifyUserAttribute | A user creates a new email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomSMSSender\_UpdateUserAttribute | A user updates an email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomSMSSender\_Authentication | A user configured with SMS multi\-factor authentication \(MFA\) signs in\. | 
| CustomSMSSender\_AdminCreateUser | You create a new user in your user pool and Amazon Cognito sends them a temporary password\. | 