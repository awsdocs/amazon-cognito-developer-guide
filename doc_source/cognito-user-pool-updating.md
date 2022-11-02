# Updating user pool configuration<a name="cognito-user-pool-updating"></a>

To change the settings of Amazon Cognito user pools in the AWS Management Console, navigate through the feature\-based tabs in your user pool settings and update fields as described in other areas of this guide\. You can't change some settings after you create a user pool\. If you want to change the following settings, you must create a new user pool or app client\.

**User pool name**  
The friendly name that you assigned to your user pool\. To change the name of a user pool, create a new user pool\.

**Amazon Cognito user pool sign\-in options**  
The attributes that your users can pass as a user name when they sign in\. When you create a user pool, you can choose to allow sign\-in with user name, email address, phone number, or a preferred user name\. To change user pool sign\-in options, create a new user pool\.

**Make user name case sensitive**  
When you create a user name that matches another user name except for the letter case, Amazon Cognito can treat them as either the same user or as unique users\. For more information, see [User pool case sensitivity](user-pool-case-sensitivity.md)\. To change case sensitivity, create a new user pool\.

**Required attributes**  
The attributes that your users must provide values for when they sign up, or when you create them\. For more information, see [User pool attributes](user-pool-settings-attributes.md)\. To change required attributes, create a new user pool\.

**Client secret**  
When you create an app client, you can generate a client secret so that only trusted sources can make requests to your user pool\. For more information, see [Configuring a user pool app client](user-pool-settings-client-apps.md)\. To change a client secret, create a new app client in the same user pool\.

**Custom attributes**  
Attributes with custom names\. You can change the value of a user's custom attribute, but you can't delete a custom attribute from your user pool\. For more information, see [User pool attributes](user-pool-settings-attributes.md)\. If you reach the maximum number of custom attributes and you want to modify the list, create a new user pool\.

## Updating a user pool with the Amazon Cognito API or AWS CLI<a name="cognito-user-pool-updating-api-cli"></a>

You can change the configuration of an Amazon Cognito user pool with automation tools like the Amazon Cognito API or AWS Command Line Interface \(AWS CLI\)\. The following procedure updates your configuration with the [ UpdateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPool.html) API operation\. The same approach, with different input fields, applies to [ UpdateUserPoolClient](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolClient.html)\.

**Important**  
If you don't provide values for existing parameters, Amazon Cognito sets them to default values\. For example, when you have existing `LambdaConfig` and you submit an `UpdateUserPool` with an empty `LambdaConfig`, you delete the assignment of all Lambda functions to user pool triggers\. Plan accordingly when you want to automate changes to your user pool configuration\.

1. Capture the existing state of your user pool with [ DescribeUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DescribeUserPool.html)\.

1. Format the output of `DescribeUserPool` to match the [ request parameters](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPool.html#API_UpdateUserPool_RequestSyntax) of `UpdateUserPool`\. Remove the following top\-level fields and their child objects from the output JSON\.
   + `Arn`
   + `CreationDate`
   + `CustomDomain`
     + Update this field with the [UpdateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolDomain.html) API operation\.
   + `Domain`
     + Update this field with the [UpdateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolDomain.html) API operation\.
   + `EmailConfigurationFailure`
   + `EstimatedNumberOfUsers`
   + `Id`
   + `LastModifiedDate`
   + `Name`
   + `SchemaAttributes`
   + `SmsConfigurationFailure`
   + `Status`

1. Confirm that the resulting JSON matches the [ request parameters](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPool.html#API_UpdateUserPool_RequestSyntax) of `UpdateUserPool`\.

1. Modify any parameters that you want to change in the resulting JSON\.

1. Submit an `UpdateUserPool` API request with your modified JSON as the request input\.

You can also use this modified `DescribeUserPool` output in the `--cli-input-json` parameter of `update-user-pool` in the AWS CLI\.

Alternately, run the following AWS CLI command to generate JSON with blank values for the accepted input fields for `update-user-pool`\. You can then populate these fields with the existing values from your user pool\.

```
aws cognito-idp update-user-pool --generate-cli-skeleton --output json
```

Run the following command to generate the same JSON object for an app client\.

```
aws cognito-idp update-user-pool-client --generate-cli-skeleton --output json
```