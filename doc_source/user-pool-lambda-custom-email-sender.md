# Custom email sender Lambda trigger<a name="user-pool-lambda-custom-email-sender"></a>

Amazon Cognito invokes the custom email sender trigger so that a third\-party provider can send email notifications to your users from your AWS Lambda function code\. Amazon Cognito sends email message events as requests to a Lambda function\. The custom code of your function must then process and deliver the message\.

**Note**  
Currently, you can't assign a custom email sender trigger in the Amazon Cognito console\. You can assign a trigger with the `LambdaConfig` parameter in a `CreateUserPool` or `UpdateUserPool` API request\.

Set up the custom email sender trigger as follows:

1. Create a Lambda function that you want to assign as your custom email sender trigger\.

1. Create an encryption key in AWS Key Management Service \(AWS KMS\)\. Amazon Cognito generates secrets \(temporary passwords and authorization codes\), then uses this key to encrypt them\. You can then use the AWS Encryption SDK in your Lambda function to decrypt the codes and send them to the user in plaintext\.

1. Grant Amazon Cognito service principal `cognito-idp.amazonaws.com` permission to invoke the Lambda function\.

1. Write Lambda function code that directs your email messages to custom delivery methods or third\-party providers\.

1. Update the user pool so that it uses a custom sender Lambda trigger\.

**Important**  
When you configure a custom email sender or a custom SMS sender function for your user pool, configure a symmetric AWS KMS key for additional security\. Amazon Cognito uses your configured KMS key to encrypt codes or temporary passwords\. Amazon Cognito sends the base64 encoded ciphertext to your Lambda functions\. For more information, see [Symmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#symmetric-cmks)\.

## Custom email sender Lambda trigger parameters<a name="custom-email-sender-parameters"></a>

These are the parameters that Amazon Cognito passes to this Lambda function along with the event information in the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-syntax-shared)\.

------
#### [ JSON ]

```
{
    "request": {
        "type": "customEmailSenderRequestV1",
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

### Custom email sender request parameters<a name="custom-email-sender-request-parameters"></a>

**type**  
The request version\. For a custom email sender event, the value of this string is always `customEmailSenderRequestV1`\.

**code**  
The encrypted code that your function can decrypt and send to your user\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the custom email sender Lambda function trigger\. To pass this data to your Lambda function, you can use the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API actions\. Amazon Cognito doesn't include data from the ClientMetadata parameter in [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) and [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) API operations in the request that it passes to the post authentication function\.

**userAttributes**  
One or more key\-value pairs that represent user attributes\.

### Custom email sender response parameters<a name="custom-email-sender-response-parameters"></a>

Amazon Cognito doesn't expect any additional return information in the custom email sender response\. Your function can use API operations to query and modify your resources, or record event metadata to an external system\.

## Activating the custom email sender Lambda trigger<a name="enable-custom-email-sender-lambda-trigger"></a>

To set up a custom email sender trigger that uses custom logic to send email messages for your user pool, activate the trigger as follows\.

**Step 1: Create a Lambda function**  
Create a Lambda function for the custom email sender trigger\. Amazon Cognito uses the [AWS encryption SDK](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction.html) to encrypt the secrets \(temporary passwords or authorization codes\)\.

**Step 2: Create an encryption key in AWS KMS**  
Create an encryption key in AWS KMS\. Amazon Cognito uses this key to encrypt temporary passwords and authorization codes that Amazon Cognito generates\. You can then decrypt these secrets in the custom sender Lambda function and send them to the user in plaintext\.

**Step 3: Grant Amazon Cognito permission to invoke the Lambda function**  
Use the following command:

```
        aws lambda add-permission --function-name lambda_arn --statement-id "CognitoLambdaInvokeAccess" --action lambda:InvokeFunction --principal cognito-idp.amazonaws.com
```

**Step 4: Edit the code to use custom sender**  
Amazon Cognito uses AWS Encryption SDK to encrypt secrets \(temporary passwords and authorization codes\) before Amazon Cognito sends the secrets to the custom sender Lambda function\. Decrypt these secrets before you send them to users\. You can choose a custom provider to send the email message\. To use the AWS Encryption SDK with your Lambda function, you must package the SDK with your function\. For more information, see [Installing the AWS encryption SDK for JavaScript](encryption-sdk/latest/developer-guide/javascript-installation.html)\. You can also update the Lambda package as follows:

1. Export the Lambda function package from the console\.

1. Unzip the package\.

1. Add the AWS Encryption SDK to the package\. For example, if you are using Node\.js, then add the `node_modules` directory and include the libraries from @aws\-crypto/client\-node\.

1. Recreate the package\.

1. Update the Lambda function code from the modified directory\.

**Step 5: Update user pool to add custom sender Lambda triggers**  
Update the user pool with a `CustomEmailSender` parameter in an `UpdateUserPool` API operation\. `UpdateUserPool` requires all the parameters of your user pool as well as the parameters that you want to change\. If you don't provide all relevant parameters, Amazon Cognito sets the values of any missing parameters to their defaults\. For more information, see [Updating user pool configuration](cognito-user-pool-updating.md)\.

```
    #Send this parameter in an 'aws cognito-idp update-user-pool' CLI command, along with any existing user pool configurations.
              
       --lambda-config "CustomEmailSender={LambdaVersion=V1_0,LambdaArn= lambda-arn },KMSKeyID= key-id"
```

To remove a custom email sender Lambda trigger with the AWS CLI, omit the `CustomEmailSender` parameter from `--lambda-config` and include all other triggers that you want to use with your user pool\. To remove a custom email sender Lambda trigger with an `UpdateUserPool` API request, remove `CustomemailSender` from the request body that contains the rest of your user pool configuration\. For more information, see [Updating a user pool with the Amazon Cognito API or AWS CLI](cognito-user-pool-updating.md#cognito-user-pool-updating-api-cli)\.

The following Node\.js example shows how to process an email message event in your custom email sender Lambda function\. This example assumes your function has two environment variables defined\.

**`KEY_ALIAS`**  
The [alias](https://docs.aws.amazon.com/kms/latest/developerguide/kms-alias.html) of the KMS key that you want to use to encrypt and decrypt your users' codes\.

**`KEY_ARN`**  
The Amazon Resource Name \(ARN\) of the KMS key that you want to use to encrypt and decrypt your users' codes\.

```
            const AWS = require('aws-sdk');
            const b64 = require('base64-js');
            const encryptionSdk = require('@aws-crypto/client-node');
            
            
            #Configure the encryption SDK client with the KMS key from the environment variables.
            
            const { encrypt, decrypt } = encryptionSdk.buildClient(encryptionSdk.CommitmentPolicy.REQUIRE_ENCRYPT_ALLOW_DECRYPT);
            const generatorKeyId = process.env.KEY_ALIAS;
            const keyIds = [ process.env.KEY_ARN ];
            const keyring = new encryptionSdk.KmsKeyringNode({ generatorKeyId, keyIds })
            exports.handler = async (event) => {

            
            #Decrypt the secret code using encryption SDK.
            let plainTextCode;
            if(event.request.code){
            const { plaintext, messageHeader } = await decrypt(keyring, b64.toByteArray(event.request.code));
            plainTextCode = plaintext
            }

            #PlainTextCode now has the decrypted secret.
            
            if(event.triggerSource == 'CustomEmailSender_SignUp'){
            
            #Send email to end-user using custom or 3rd party provider.
            #Include temporary password in the email.
            
            }else if(event.triggerSource == 'CustomEmailSender_ResendCode'){
            
            }else if(event.triggerSource == 'CustomEmailSender_ForgotPassword'){
            
            }else if(event.triggerSource == 'CustomEmailSender_UpdateUserAttribute'){
            
            }else if(event.triggerSource == 'CustomEmailSender_VerifyUserAttribute'){
            
            }else if(event.triggerSource == 'CustomEmailSender_AdminCreateUser'){
            
            }else if(event.triggerSource == 'CustomEmailSender_AccountTakeOverNotification'){
            
            }
            
            return;
            };
```

## Custom email sender Lambda trigger sources<a name="trigger-source"></a>

The following table shows the triggering events for custom email trigger sources in your Lambda code\.


| `TriggerSource value` | Triggering event | 
| --- | --- | 
| CustomEmailSender\_SignUp | A user signs up and Amazon Cognito sends a welcome message\. | 
| CustomEmailSender\_ForgotPassword | A user requests a code to reset their password\. | 
| CustomEmailSender\_ResendCode | A user requests a replacement code to reset their password\. | 
| CustomEmailSender\_UpdateUserAttribute | A user updates an email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomEmailSender\_VerifyUserAttribute | A user creates a new email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomEmailSender\_AdminCreateUser | You create a new user in your user pool and Amazon Cognito sends them a temporary password\. | 
| CustomEmailSender\_AccountTakeOverNotification | Amazon Cognito detects an attempt to take over a user account and sends the user a notification\. | 