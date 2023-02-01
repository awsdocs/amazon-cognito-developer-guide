# List Amazon Cognito Identity pools<a name="example_cognito-identity_ListIdentityPools_section"></a>

The following code example shows how to get a list of Amazon Cognito Identity pools\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Swift ]

**SDK for Swift**  
This is prerelease documentation for an SDK in preview release\. It is subject to change\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/swift/example_code/cognito-identity/FindOrCreateIdentityPool#code-examples)\. 
Find the ID of an identity pool given its name\.  

```
    func getIdentityPoolID(name: String) async throws -> String? {
        var token: String? = nil
        
        // Iterate over the identity pools until a match is found.
        repeat {
            /// `token` is a value returned by `ListIdentityPools()` if the returned list
            /// of identity pools is only a partial list. You use the `token` to tell Cognito that
            /// you want to continue where you left off previously; specifying `nil` or not providing
            /// it means "start at the beginning."
            
            let listPoolsInput = ListIdentityPoolsInput(maxResults: 25, nextToken: token)
            
            /// Read pages of identity pools from Cognito until one is found
            /// whose name matches the one specified in the `name` parameter.
            /// Return the matching pool's ID. Each time we ask for the next
            /// page of identity pools, we pass in the token given by the
            /// previous page.
            
            do {
                let output = try await cognitoIdentityClient.listIdentityPools(input: listPoolsInput)

                if let identityPools = output.identityPools {
                    for pool in identityPools {
                        if pool.identityPoolName == name {
                            return pool.identityPoolId!
                        }
                    }
                }
                
                token = output.nextToken
            } catch {
                print("ERROR: ", dump(error, name: "Trying to get list of identity pools"))
            }
        } while token != nil
        
        return nil
    }
```
Get the ID of an existing identity pool or create it if it doesn't already exist\.  

```
    public func getOrCreateIdentityPoolID(name: String) async throws -> String? {
        // See if the pool already exists
        
        do {
            guard let poolId = try await self.getIdentityPoolID(name: name) else {
                let poolId = try await self.createIdentityPool(name: name)
                return poolId
            }
      
            return poolId
           
        } catch {
            print("ERROR: ", dump(error, name: "Trying to get the identity pool ID"))
            return nil
        }
    }
```
+  For more information, see [AWS SDK for Swift developer guide](https://docs.aws.amazon.com/sdk-for-swift/latest/developer-guide/getting-started.html)\. 
+ For API details, see the following topics in *AWS SDK for Swift API reference*\.
  + [createIdentityPool](https://awslabs.github.io/aws-sdk-swift/reference/0.x)
  + [listIdentityPools](https://awslabs.github.io/aws-sdk-swift/reference/0.x)

------

For a complete list of AWS SDK developer guides and code examples, see [Using this service with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.