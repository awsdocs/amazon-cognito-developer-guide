# Pre token generation Lambda trigger<a name="user-pool-lambda-pre-token-generation"></a>

Amazon Cognito invokes this trigger before token generation which allows you to customize identity token claims\.

This Lambda trigger allows you to customize an identity token before it is generated\. You can use this trigger to add new claims, update claims, or suppress claims in the identity token\. To use this feature, associate a Lambda function from the Amazon Cognito user pools console or update your user pool through the AWS CLI\.

You can not modify the following claims:
+ `acr`
+ `amr`
+ `aud`
+ `at_hash`
+ `auth_time`
+ `azp`
+ `cognito:username`
+ `exp`
+ `iat`
+ `identities`
+ `iss`
+ `jti`
+ `nbf`
+ `nonce`
+ `origin_jti`
+ `sub`
+ `token_use`

**Topics**
+ [Pre token generation Lambda trigger sources](#user-pool-lambda-pre-token-generation-trigger-source)
+ [Pre token generation Lambda trigger parameters](#cognito-user-pools-lambda-trigger-syntax-pre-token-generation)
+ [Pre token generation example: Add a new claim and suppress an existing claim](#aws-lambda-triggers-pre-token-generation-example-1)
+ [Pre token generation example: Modify the user's group membership](#aws-lambda-triggers-pre-token-generation-example-2)

## Pre token generation Lambda trigger sources<a name="user-pool-lambda-pre-token-generation-trigger-source"></a>


| triggerSource value | Triggering event | 
| --- | --- | 
| TokenGeneration\_HostedAuth | Called during authentication from the Amazon Cognito hosted UI sign\-in page\. | 
| TokenGeneration\_Authentication | Called after user authentication flows have completed\. | 
| TokenGeneration\_NewPasswordChallenge | Called after the user is created by an admin\. This flow is invoked when the user has to change a temporary password\. | 
| TokenGeneration\_AuthenticateDevice | Called at the end of the authentication of a user device\. | 
| TokenGeneration\_RefreshTokens | Called when a user tries to refresh the identity and access tokens\. | 

## Pre token generation Lambda trigger parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-token-generation"></a>

These are the parameters required by this Lambda function in addition to the [common parameters](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools-working-with-aws-lambda-triggers.html#cognito-user-pools-lambda-trigger-sample-event-parameter-shared)\.

------
#### [ JSON ]

```
{
    "request": {
        "userAttributes": {"string": "string"},
        "groupConfiguration": [
            {
                "groupsToOverride": [
                    "string",
                    "string"
                ],
                "iamRolesToOverride": [
                    "string",
                    "string"
                ],
                "preferredRole": "string"
            }
        ],
        "clientMetadata": {"string": "string"}
    },
    "response": {
        "claimsOverrideDetails": {
            "claimsToAddOrOverride": {"string": "string"},
            "claimsToSuppress": [
                "string",
                "string"
            ],
            "groupOverrideDetails": {
                "groupsToOverride": [
                    "string",
                    "string"
                ],
                "iamRolesToOverride": [
                    "string",
                    "string"
                ],
                "preferredRole": "string"
            }
        }
    }
}
```

------

### Pre token generation request parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-token-generation-request"></a>

**groupConfiguration**  
The input object containing the current group configuration\. It includes `groupsToOverride`, `iamRolesToOverride`, and `preferredRole`\.

**groupsToOverride**  
A list of the group names that are associated with the user that the identity token is issued for\.

**iamRolesToOverride**  
A list of the current IAM roles associated with these groups\.

**preferredRole**  
A string indicating the preferred IAM role\.

**clientMetadata**  
One or more key\-value pairs that you can provide as custom input to the Lambda function that you specify for the pre token generation trigger\. You can pass this data to your Lambda function by using the ClientMetadata parameter in the [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminRespondToAuthChallenge.html) and [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) API actions\.

### Pre token generation response parameters<a name="cognito-user-pools-lambda-trigger-syntax-pre-token-generation-response"></a>

**claimsToAddOrOverride**  
A map of one or more key\-value pairs of claims to add or override\. For group related claims, use groupOverrideDetails instead\.

**claimsToSuppress**  
A list that contains claims to be suppressed from the identity token\.  
If a value is both suppressed and replaced, then it will be suppressed\.

**groupOverrideDetails**  
The output object containing the current group configuration\. It includes `groupsToOverride`, `iamRolesToOverride`, and `preferredRole`\.  
The groupOverrideDetails object is replaced with the one you provide\. If you provide an empty or null object in the response, then the groups are suppressed\. To leave the existing group configuration as is, copy the value of the request's groupConfiguration object to the groupOverrideDetails object in the response, and pass it back to the service\.  
Amazon Cognito ID and access tokens both contain the `cognito:groups` claim\. Your groupOverrideDetails object will replace the `cognito:groups` claim in access tokens as well as ID tokens\.

## Pre token generation example: Add a new claim and suppress an existing claim<a name="aws-lambda-triggers-pre-token-generation-example-1"></a>

This example uses the Pre Token Generation Lambda to add a new claim and suppresses an existing one\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    event.response = {
        "claimsOverrideDetails": {
            "claimsToAddOrOverride": {
                "attribute_key2": "attribute_value2",
                "attribute_key": "attribute_value"
            },
            "claimsToSuppress": ["email"]
        }
    };

    // Return to Amazon Cognito
    callback(null, event);
};
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object back to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
  "request": {},
  "response": {}
}
```

------

## Pre token generation example: Modify the user's group membership<a name="aws-lambda-triggers-pre-token-generation-example-2"></a>

This example uses the Pre Token Generation Lambda to modify the user's group membership\.

------
#### [ Node\.js ]

```
exports.handler = (event, context, callback) => {
    event.response = {
        "claimsOverrideDetails": {
            "claimsToAddOrOverride": {
                "attribute_key2": "attribute_value2",
                "attribute_key": "attribute_value"
            },
            "claimsToSuppress": ["email"],
            "groupOverrideDetails": {
                "groupsToOverride": ["group-A", "group-B", "group-C"],
                "iamRolesToOverride": ["arn:aws:iam::XXXXXXXXXXXX:role/sns_callerA", "arn:aws:iam::XXXXXXXXX:role/sns_callerB", "arn:aws:iam::XXXXXXXXXX:role/sns_callerC"],
                "preferredRole": "arn:aws:iam::XXXXXXXXXXX:role/sns_caller"
            }
        }
    };

    // Return to Amazon Cognito
    callback(null, event);
};
```

------

Amazon Cognito passes event information to your Lambda function\. The function then returns the same event object back to Amazon Cognito, with any changes in the response\. In the Lambda console, you can set up a test event with data that’s relevant to your Lambda trigger\. The following is a test event for this code sample:

------
#### [ JSON ]

```
{
  "request": {},
  "response": {}
}
```

------