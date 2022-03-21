# Accessing AWS services<a name="accessing-aws-services"></a>

 Once the Amazon Cognito credentials provider is initialized and refreshed, you can pass it directly to the initializer for an AWS client\. For example, the following snippet initializes an Amazon DynamoDB client: 

## Android<a name="accessing-aws-services-1.android"></a>



```
// Create a service client with the provider
AmazonDynamoDB client = new AmazonDynamoDBClient(credentialsProvider);
```

 The credentials provider communicates with Amazon Cognito, retrieving both the unique identifier for authenticated and unauthenticated users as well as temporary, limited privilege AWS credentials for the AWS Mobile SDK\. The retrieved credentials are valid for one hour, and the provider refreshes them when they expire\. 

## iOS \- Objective\-C<a name="accessing-aws-services-1.ios-objc"></a>

```
// create a configuration that uses the provider
AWSServiceConfiguration *configuration = [AWSServiceConfiguration configurationWithRegion:AWSRegionUSEast1 provider:credentialsProvider];

// get a client with the default service configuration
AWSDynamoDB *dynamoDB = [AWSDynamoDB defaultDynamoDB];
```

 The credentials provider communicates with Amazon Cognito, retrieving both the unique identifier for authenticated and unauthenticated users as well as temporary, limited privilege AWS credentials for the AWS Mobile SDK\. The retrieved credentials are valid for one hour, and the provider refreshes them when they expire\. 

## iOS \- Swift<a name="accessing-aws-services-1.ios-swift"></a>

```
// get a client with the default service configuration
let dynamoDB = AWSDynamoDB.default()

// get a client with a custom configuration
AWSDynamoDB.register(with: configuration!, forKey: "USWest2DynamoDB");
let dynamoDBCustom = AWSDynamoDB(forKey: "USWest2DynamoDB")
```

 The credentials provider communicates with Amazon Cognito, retrieving both the unique identifier for authenticated and unauthenticated users as well as temporary, limited privilege AWS credentials for the AWS Mobile SDK\. The retrieved credentials are valid for one hour, and the provider refreshes them when they expire\. 

## JavaScript<a name="accessing-aws-services-1.javascript"></a>



```
// Create a service client with the provider
var dynamodb = new AWS.DynamoDB({region: 'us-west-2'});
```

 The credentials provider communicates with Amazon Cognito, retrieving both the unique identifier for authenticated and unauthenticated users as well as temporary, limited\-privilege AWS credentials for the AWS Mobile SDK\. The retrieved credentials are valid for one hour, and the provider refreshes them when they expire\. 

## Unity<a name="accessing-aws-services-1.unity"></a>



```
// create a service client that uses credentials provided by Cognito
AmazonDynamoDBClient client = new AmazonDynamoDBClient(credentials, REGION);
```

 The credentials provider communicates with Amazon Cognito, retrieving both the unique identifier for authenticated and unauthenticated users as well as temporary, limited\-privilege AWS credentials for the AWS Mobile SDK\. The retrieved credentials are valid for one hour, and the provider refreshes them when they expire\. 

## Xamarin<a name="accessing-aws-services-1.xamarin"></a>



```
// create a service client that uses credentials provided by Cognito
var client = new AmazonDynamoDBClient(credentials, REGION)
```

 The credentials provider communicates with Amazon Cognito, retrieving both the unique identifier for authenticated and unauthenticated users as well as temporary, limited\-privilege AWS credentials for the AWS Mobile SDK\. The retrieved credentials are valid for one hour, and the provider refreshes them when they expire\. 