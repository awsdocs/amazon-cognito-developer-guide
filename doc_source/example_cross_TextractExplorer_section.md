# Create an Amazon Textract explorer application<a name="example_cross_TextractExplorer_section"></a>

The following code examples show how to explore Amazon Textract output through an interactive application\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ JavaScript ]

**SDK for JavaScript V3**  
 Shows how to use the AWS SDK for JavaScript to build a React application that uses Amazon Textract to extract data from a document image and display it in an interactive web page\. This example runs in a web browser and requires an authenticated Amazon Cognito identity for credentials\. It uses Amazon Simple Storage Service \(Amazon S3\) for storage, and for notifications it polls an Amazon Simple Queue Service \(Amazon SQS\) queue that is subscribed to an Amazon Simple Notification Service \(Amazon SNS\) topic\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cross-services/textract-react)\.   

**Services used in this example**
+ Amazon Cognito Identity
+ Amazon S3
+ Amazon SNS
+ Amazon SQS
+ Amazon Textract

------

For a complete list of AWS SDK developer guides and code examples, see [Using this service with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.