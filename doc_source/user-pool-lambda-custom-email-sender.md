# Custom email Lambda trigger<a name="user-pool-lambda-custom-email-sender"></a>

Amazon Cognito invokes the `CustomEmailSender` trigger so that a third\-party provider can send email notifications to your users from within your Lambda function code\. Use this trigger as follows:

**Note**  
The `CustomEmailSender` trigger isn't available in the Amazon Cognito console\.

1. Create a Lambda function for the `CustomEmailSender`\. Amazon Cognito uses the AWS Encryption SDK to encrypt the secrets \(temporary passwords or authorization codes\)\.

1. Create an encryption key in AWS KMS\. Amazon Cognito uses this key to encrypt temporary passwords and authorization codes that Amazon Cognito generates\. You can then decrypt these secrets in the custom sender Lambda function and send them to the user in plaintext\.

1. Grant Amazon Cognito service principal cognito\-idp\.amazonaws\.com permission to invoke the Lambda function\.

1. Write code that uses custom senders or third\-party providers to your Lambda function\.

1. Update the user pool so that it uses a custom sender Lambda trigger\.

**Important**  
When you configure `CustomEmailSender` or `CustomSMSSender` with your user pool, you must configure a symmetric AWS KMS key for additional security\. Amazon Cognito uses your configured KMS key to encrypt codes or temporary passwords\. Amazon Cognito sends the base64 encoded ciphertext to your Lambda functions\. For more information, see [Symmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#symmetric-cmks)\.

## Activate the `CustomEmailSender` Lambda trigger<a name="enable-custom-email-sender-lambda-trigger"></a>

The `CustomEmailSender` trigger uses a Lambda function\.

**Step 1: Create a Lambda function**  
Create a Lambda function for the `CustomEmailSender` trigger\. Amazon Cognito uses the [AWS encryption SDK](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction.html) to encrypt the secrets \(temporary passwords or authorization codes\)\.

**Step 2: Create an encryption key in AWS KMS**  
Create an encryption key in AWS KMS\. Amazon Cognito uses this key to encrypt temporary passwords and authorization codes that Amazon Cognito generates\. You can then decrypt these secrets in the custom sender Lambda function and send them to the user in plaintext\.

**Step 3: Grant Amazon Cognito service principal cognito\-idp\.amazonaws\.com permission to invoke the Lambda function**  
Use the following command:

```
        aws lambda add-permission --function-name lambda_arn --statement-id "CognitoLambdaInvokeAccess" --action lambda:InvokeFunction --principal cognito-idp.amazonaws.com
```

**Step 4: Edit the code to use custom sender**  
Amazon Cognito uses AWS Encryption SDK to encrypt secrets \(temporary passwords and authorization codes\) before it sends the secrets to the custom sender Lambda function\. Decrypt these secrets before you send them to users\. You can choose a custom provider to send the email message\. To use the AWS Encryption SDK with your Lambda function, you must package the SDK with your function\. For more information, see [Installing the AWS encryption SDK for JavaScript](encryption-sdk/latest/developer-guide/javascript-installation.html)\. You can also update the Lambda package as follows:

1. Export the Lambda function package from the console\.

1. Unzip the package\.

1. Add the AWS Encryption SDK to the package\. For example, if you are using Node\.js, then add the `node_modules` directory and include the libraries from @aws\-crypto/client\-node\.

1. Recreate the package\.

1. Update the Lambda function code from the modified directory\.

**Step 5: Update user pool to add custom sender Lambda triggers**  
Update the user pool to add the `CustomEmailSender` trigger\.

```
              #Send the parameter to update-user-pool along with any existing user pool configurations.
              
                --lambda-config "CustomEmailSender={LambdaVersion=V1_0,LambdaArn= lambda-arn },KMSKeyID= key-id"
```

The following Node\.js example shows how to use the `CustomEmailSender` Lambda function\.

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
| CustomEmailSender\_ResendCode | A user requests a replacement code to reset their password\. | 
| CustomEmailSender\_ForgotPassword | A user requests a code to reset their password\. | 
| CustomEmailSender\_UpdateUserAttribute | A user updates an email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomEmailSender\_VerifyUserAttribute | A user creates a new email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomEmailSender\_AdminCreateUser | You create a new user in your user pool and Amazon Cognito sends them a temporary password\. | 
| CustomEmailSender\_AccountTakeOverNotification | Amazon Cognito detects an attempt to take over a user account and sends the user a notification\. | 