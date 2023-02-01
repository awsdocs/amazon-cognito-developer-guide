# Create a new Amazon Cognito Identity with a specified name<a name="example_cognito-identity_CreateIdentityPool_section"></a>

The following code example shows how to find the Amazon Cognito Identity with the given name, creating it if it's not found\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Swift ]

**SDK for Swift**  
This is prerelease documentation for an SDK in preview release\. It is subject to change\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/swift/example_code/cognito-identity/FindOrCreateIdentityPool#code-examples)\. 
Create a new identity pool\.  

```
    func createIdentityPool(name: String) async throws -> String? {
        let cognitoInputCall = CreateIdentityPoolInput(developerProviderName: "com.exampleco.CognitoIdentityDemo",
                                                       identityPoolName: name)

        do {
            let result = try await cognitoIdentityClient.createIdentityPool(input: cognitoInputCall)
            guard let poolId = result.identityPoolId  else {
                return nil
            }
            
            return poolId
            
        } catch {
            print("ERROR: ", dump(error, name: "Error attempting to create the identity pool"))
        }
        
        return nil
    }
```
+  For more information, see [AWS SDK for Swift developer guide](https://docs.aws.amazon.com/sdk-for-swift/latest/developer-guide/getting-started.html)\. 
+ For API details, see the following topics in *AWS SDK for Swift API reference*\.
  + [createIdentityPool](https://awslabs.github.io/aws-sdk-swift/reference/0.x)
  + [listIdentityPools](https://awslabs.github.io/aws-sdk-swift/reference/0.x)

------

For a complete list of AWS SDK developer guides and code examples, see [Using this service with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.