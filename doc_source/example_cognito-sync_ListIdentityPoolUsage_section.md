# Get a list of identity pools registered with Amazon Cognito using an AWS SDK<a name="example_cognito-sync_ListIdentityPoolUsage_section"></a>

The following code example shows how to list Amazon Cognito identity pools\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Rust ]

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/cognitosync#code-examples)\. 
  

```
async fn show_pools(client: &Client) -> Result<(), Error> {
    let response = client
        .list_identity_pool_usage()
        .max_results(10)
        .send()
        .await?;

    if let Some(pools) = response.identity_pool_usages() {
        println!("Identity pools:");

        for pool in pools {
            println!(
                "  Identity pool ID:    {}",
                pool.identity_pool_id().unwrap_or_default()
            );
            println!(
                "  Data storage:        {}",
                pool.data_storage().unwrap_or_default()
            );
            println!(
                "  Sync sessions count: {}",
                pool.sync_sessions_count().unwrap_or_default()
            );
            println!(
                "  Last modified:       {}",
                pool.last_modified_date().unwrap().to_chrono_utc()?
            );
            println!();
        }
    }

    println!("Next token: {}", response.next_token().unwrap_or_default());

    Ok(())
}
```
+  For API details, see [ListIdentityPoolUsage](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using this service with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.