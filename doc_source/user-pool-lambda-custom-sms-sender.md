# Custom SMS sender Lambda trigger<a name="user-pool-lambda-custom-sms-sender"></a>

Amazon Cognito invokes the `CustomSMSSender` trigger from within your Lambda function code so that a third\-party provider can send SMS notifications to your users\. To use this trigger, perform the following steps:

**Note**  
The `CustomSMSSender` trigger isn't available in the Amazon Cognito console\.

1. Create a Lambda function for the `CustomSMSSender`\.

1. Create an encryption key in AWS KMS\.

1. Grant Amazon Cognito service principal cognito\-idp\.amazonaws\.com access to invoke the Lambda function\.

1. Edit the code in your Lambda function to include third\-party providers\.

1. Update your user pool to add custom triggers\. 

**Important**  
For additional security, you must configure a symmetric AWS KMS key when you configure `CustomEmailSender` or `CustomSMSSender` with your user pool\. Amazon Cognito uses your configured KMS key to encrypt codes or temporary passwords\. Amazon Cognito sends the base64 encoded ciphertext to your Lambda functions\. For more information see, [Symmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#symmetric-cmks)\.

## Enable the `CustomSMSSender` Lambda trigger<a name="enable-custom-sms-sender-lambda-trigger"></a>

You can enable the `CustomSMSSender` trigger using a Lambda function\.

**Step 1: Create a Lambda function**  
 Create a Lambda function for the `CustomSMSSender` trigger\. Amazon Cognito uses the [AWS encryption SDK](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/introduction.html) to encrypt the secrets \(temporary passwords or authorization codes\)\.

**Step 2: Create an encryption key in AWS KMS**  
Create an encryption key in AWS KMS\. This key encrypts temporary passwords and authorization codes that Amazon Cognito generates\. You can then decrypt these secrets in the custom sender Lambda function and send them to the user in plaintext\.

**Step 3: Grant Amazon Cognito service principal cognito\-idp\.amazonaws\.com access to invoke the Lambda function**  
Use the following command to grant access to the Lambda function:

```
        aws lambda add-permission --function-name lambda_arn --statement-id "CognitoLambdaInvokeAccess" --action lambda:InvokeFunction --principal cognito-idp.amazonaws.com
```

**Step 4: Edit the code to use custom sender**  
Amazon Cognito uses AWS Encryption SDK to encrypt secrets \(temporary passwords and authorization codes\) before it sends the secrets to the custom sender Lambda function\. Decrypt these secrets before you send them to users through the custom provider of your choice\. To use the AWS Encryption SDK with your Lambda function, you must package the SDK with your function\. For information, see [Installing the AWS encryption SDK for JavaScript](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/javascript-installation.html)\. To update the Lambda package, complete the following steps:

1. Export the Lambda function package from the console\.

1. Unzip the package\.

1. Add the AWS Encryption SDK to the package\. For example, if you are using Node\.js, then add the `node_modules` directory and include the libraries from @aws\-crypto/client\-node\.

1. Recreate the package\.

1. Update the Lambda function code from the modified directory\.

**Step 5: Update user pool to add custom sender Lambda triggers**  
Update the user pool to add the CustomSMSSender trigger\.

## Code examples<a name="code-examples"></a>

```
    #Send the parameter to update-user-pool along with any existing user pool configurations.
              
      --lambda-config "CustomSMSSender={LambdaVersion=V1_0,LambdaArn= lambda-arn },KMSKeyID= key-id"
```

 The following Node\.js example shows how to use the `CustomSMSSender` Lambda function\.

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
+ [Enable the `CustomSMSSender` Lambda trigger](#enable-custom-sms-sender-lambda-trigger)
+ [Code examples](#code-examples)
+ [Custom SMS sender Lambda trigger sources](#trigger-source)

## Custom SMS sender Lambda trigger sources<a name="trigger-source"></a>

The following table shows the triggering event for custom SMS trigger sources in your Lambda code\.


| `TriggerSource value` | Triggering event | 
| --- | --- | 
| CustomSMSSender\_SignUp | A user signs up and Amazon Cognito sends a welcome message\. | 
| CustomSMSSender\_ResendCode | A user requests a replacement code to reset their password\. | 
| CustomSMSSender\_ForgotPassword | A user requests a code to reset their password\. | 
| CustomSMSSender\_UpdateUserAttribute | A user updates an email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomSMSSender\_VerifyUserAttribute | A user creates a new email address or phone number attribute and Amazon Cognito sends a code to verify the attribute\. | 
| CustomSMSSender\_Authentication | A user with SMS MFA configured signs in\. | 
| CustomSMSSender\_AdminCreateUser | You create a new user in your user pool and Amazon Cognito sends them a temporary password\. | 