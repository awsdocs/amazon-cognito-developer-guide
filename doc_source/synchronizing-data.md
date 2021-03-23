# Synchronizing Data<a name="synchronizing-data"></a>

****  
If you're new to Amazon Cognito Sync, use [AWS AppSync](https://aws.amazon.com/appsync/)\. Like Amazon Cognito Sync, AWS AppSync is a service for synchronizing application data across devices\.  
It enables user data like app preferences or game state to be synchronized\. It also extends these capabilities by allowing multiple users to synchronize and collaborate in real time on shared data\.

Amazon Cognito lets you save end user data in datasets containing key\-value pairs\. This data is associated with an Amazon Cognito identity, so that it can be accessed across logins and devices\. To sync this data between the Amazon Cognito service and an end user’s devices, invoke the synchronize method\. Each dataset can have a maximum size of 1 MB\. You can associate up to 20 datasets with an identity\.

The Amazon Cognito Sync client creates a local cache for the identity data\. Your app talks to this local cache when it reads and writes keys\. This guarantees that all of your changes made on the device are immediately available on the device, even when you are offline\. When the synchronize method is called, changes from the service are pulled to the device, and any local changes are pushed to the service\. At this point the changes are available to other devices to synchronize\.

## Initializing the Amazon Cognito Sync Client<a name="initializing-client"></a>



To initialize the Amazon Cognito Sync client, you first need to create a credentials provider\. The credentials provider acquires temporary AWS credentials to enable your app to access your AWS resources\. You'll also need to import the required header files\. Use the following steps to initialize the Amazon Cognito Sync client\.

### Android<a name="initialize-cog-sync-1.android"></a>

1. Create a credentials provider, following the instructions in [Getting Credentials](getting-credentials.md)\.

1. Import the Amazon Cognito package: `import com.amazonaws.mobileconnectors.cognito.*;`

1. Initialize Amazon Cognito Sync, passing in the Android app context, the identity pool ID, an AWS region, and an initialized Amazon Cognito credentials provider:

   ```
   CognitoSyncManager client = new CognitoSyncManager(
       getApplicationContext(),
       Regions.YOUR_REGION,
       credentialsProvider);
   ```

### iOS \- Objective\-C<a name="initialize-cog-sync-1.ios-objc"></a>

1. Create a credentials provider, following the instructions in [Getting Credentials](getting-credentials.md)\.

1. Import `AWSCore` and `Cognito`, and initialize `AWSCognito`:

   ```
   #import <AWSiOSSDKv2/AWSCore.h>
   #import <AWSCognitoSync/Cognito.h>
   
   AWSCognito *syncClient = [AWSCognito defaultCognito];
   ```

1. If you're using CocoaPods, replace `<AWSiOSSDKv2/AWSCore.h>` with `AWSCore.h` and follow the same syntax for the Amazon Cognito import\.

### iOS \- Swift<a name="initialize-cog-sync-1.ios-swift"></a>

1. Create a credentials provider, following the instructions in [Getting Credentials](getting-credentials.md)\.

1. Import and initialize `AWSCognito`:

   ```
   import AWSCognito
   let syncClient = AWSCognito.default()!
   ```

### JavaScript<a name="initialize-cog-sync-1.javascript"></a>

1. Download the [Amazon Cognito Sync Manager for JavaScript](https://github.com/aws/amazon-cognito-js)\.

1. Include the Sync Manager library in your project\.

1. Create a credentials provider, following the instructions in [Getting Credentials](getting-credentials.md)\.

1. Initialize the Sync Manager:

   ```
   var syncManager = new AWS.CognitoSyncManager();
   ```

### Unity<a name="initialize-cog-sync-1.unity"></a>

1. You will need to first create an instance of `CognitoAWSCredentials`, following the instructions in [Getting Credentials](getting-credentials.md)\.

1. Create an instance of `CognitoSyncManager`, passing the `CognitoAwsCredentials` object and a `AmazonCognitoSyncConfig` with, at least, the region set: 

   ```
   AmazonCognitoSyncConfig clientConfig = new AmazonCognitoSyncConfig { RegionEndpoint = REGION };
   CognitoSyncManager syncManager = new CognitoSyncManager(credentials, clientConfig);
   ```

### Xamarin<a name="initialize-cog-sync-1.xamarin"></a>

1. You will need to first create an instance of `CognitoAWSCredentials`, following the instructions in [Getting Credentials](getting-credentials.md)\.

1. Create an instance of `CognitoSyncManager`, passing the `CognitoAwsCredentials` object and a `AmazonCognitoSyncConfig` with, at least, the region set: 

   ```
   AmazonCognitoSyncConfig clientConfig = new AmazonCognitoSyncConfig { RegionEndpoint = REGION };
   CognitoSyncManager syncManager = new CognitoSyncManager(credentials, clientConfig);
   ```

## Understanding Datasets<a name="understanding-datasets"></a>



With Amazon Cognito, end user profile data is organized into datasets\. Each dataset can contain up to 1MB of data in the form of key\-value pairs\. A dataset is the most granular entity on which you can perform a sync operation\. Read and write operations performed on a dataset only affect the local store until the synchronize method is invoked\. A dataset is identified by a unique string\. You can create a new dataset or open an existing one as shown in the following\.

### Android<a name="understanding-datasets-1.android"></a>

```
Dataset dataset = client.openOrCreateDataset("datasetname");
```

To delete a dataset, first call the method to remove it from local storage, then call the `synchronize` method to delete the dataset from Amazon Cognito:

```
dataset.delete();
dataset.synchronize(syncCallback);
```

### iOS \- Objective\-C<a name="understanding-datasets-1.ios-objc"></a>

```
AWSCognitoDataset *dataset = [syncClient openOrCreateDataset:@"myDataSet"];
```

To delete a dataset, first call the method to remove it from local storage, then call the `synchronize` method to delete the dataset from Amazon Cognito:

```
[dataset clear];
[dataset synchronize];
```

### iOS \- Swift<a name="understanding-datasets-1.ios-swift"></a>

```
let dataset = syncClient.openOrCreateDataset("myDataSet")!
```

To delete a dataset, first call the method to remove it from local storage, then call the `synchronize` method to delete the dataset from Amazon Cognito:

```
dataset.clear()
dataset.synchronize()
```

### JavaScript<a name="understanding-datasets-1.javascript"></a>

```
syncManager.openOrCreateDataset('myDatasetName', function(err, dataset) {
   // ...
});
```

### Unity<a name="understanding-datasets-1.unity"></a>

```
string myValue = dataset.Get("myKey");
dataset.Put("myKey", "newValue");
```

You can use `Remove` to delete a key from a dataset:

```
dataset.Remove("myKey");
```

### Xamarin<a name="understanding-datasets-1.xamarin"></a>

```
Dataset dataset = syncManager.OpenOrCreateDataset("myDatasetName");
```

To delete a dataset, first call the method to remove it from local storage, then call the `synchronize` method to delete the dataset from Amazon Cognito:

```
dataset.Delete();
dataset.SynchronizeAsync();
```

## Reading and Writing Data in Datasets<a name="reading-and-writing-data"></a>

Amazon Cognito datasets function as dictionaries, with values accessible by key\. The keys and values of a dataset can be read, added, or modified just as if the dataset were a dictionary\. The following shows an example\.

Note that values written to a dataset only affect the local cached copy of the data until you call the synchronize method\.

### Android<a name="reading-and-writing-data-1.android"></a>

```
String value = dataset.get("myKey");
dataset.put("myKey", "my value");
```

### iOS \- Objective\-C<a name="reading-and-writing-data-1.ios-objc"></a>

```
[dataset setString:@"my value" forKey:@"myKey"];
NSString *value = [dataset stringForKey:@"myKey"];
```

### iOS \- Swift<a name="reading-and-writing-data-1.ios-swift"></a>

```
dataset.setString("my value", forKey:"myKey")
let value = dataset.stringForKey("myKey")
```

### JavaScript<a name="reading-and-writing-data-1.javascript"></a>



```
dataset.get('myKey', function(err, value) {
  console.log('myRecord: ' + value);
});

dataset.put('newKey', 'newValue', function(err, record) {
  console.log(record);
});

dataset.remove('oldKey', function(err, record) {
  console.log(success);
});
```

### Unity<a name="reading-and-writing-data-1.unity"></a>

```
string myValue = dataset.Get("myKey");
dataset.Put("myKey", "newValue");
```

### Xamarin<a name="reading-and-writing-data-1.xamarin"></a>



```
//obtain a value
string myValue = dataset.Get("myKey");

// Create a record in a dataset and synchronize with the server
dataset.OnSyncSuccess += SyncSuccessCallback;
dataset.Put("myKey", "myValue");
dataset.SynchronizeAsync();

void SyncSuccessCallback(object sender, SyncSuccessEventArgs e) {
  // Your handler code here
}
```

### Android<a name="reading-and-writing-data-2.android"></a>

You can use the `remove` method to remove keys from a dataset:

```
dataset.remove("myKey");
```

### iOS \- Objective\-C<a name="reading-and-writing-data-2.ios-objc"></a>

You can use `removeObjectForKey` to delete a key from a dataset:

```
[dataset removeObjectForKey:@"myKey"];
```

### iOS \- Swift<a name="reading-and-writing-data-2.ios-swift"></a>

You can use `removeObjectForKey` to delete a key from a dataset:

```
dataset.removeObjectForKey("myKey")
```

### Unity<a name="reading-and-writing-data-2.unity"></a>

You can use `Remove` to delete a key from a dataset:

```
dataset.Remove("myKey");
```

### Xamarin<a name="reading-and-writing-data-2.xamarin"></a>

You can use `Remove` to delete a key from a dataset:

```
dataset.Remove("myKey");
```

## Synchronizing Local Data with the Sync Store<a name="synchronizing-local-data"></a>



### Android<a name="synchronizing-local-data-1.android"></a>

The `synchronize` method compares local cached data to the data stored in the Amazon Cognito Sync store\. Remote changes are pulled from the Amazon Cognito Sync store; conflict resolution is invoked if any conflicts occur; and updated values on the device are pushed to the service\. To synchronize a dataset, call its `synchronize` method:

```
dataset.synchronize(syncCallback);
```

The `synchronize` method receives an implementation of the `SyncCallback` interface, discussed below\.

The `synchronizeOnConnectivity()` method attempts to synchronize when connectivity is available\. If connectivity is immediately available, `synchronizeOnConnectivity()` behaves like `synchronize()`\. Otherwise it monitors for connectivity changes and performs a sync once connectivity is available\. If `synchronizeOnConnectivity()`is called multiple times, only the last synchronize request is kept, and only the last callback will fire\. If either the dataset or the callback is garbage\-collected, this method won't perform a sync, and the callback won't fire\.

To learn more about dataset synchronization and the different callbacks, see [Handling Callbacks](handling-callbacks.md)\.

### iOS \- Objective\-C<a name="synchronizing-local-data-1.ios-objc"></a>

The `synchronize` method compares local cached data to the data stored in the Amazon Cognito Sync store\. Remote changes are pulled from the Amazon Cognito Sync store; conflict resolution is invoked if any conflicts occur; and updated values on the device are pushed to the service\. To synchronize a dataset, call its `synchronize` method:

The `synchronize` method is asynchronous and returns an `AWSTask` object to handle the response:

```
[[dataset synchronize] continueWithBlock:^id(AWSTask *task) {
    if (task.isCancelled) {
        // Task cancelled.
    } else if (task.error) {
        // Error while executing task.
    } else {
        // Task succeeded. The data was saved in the sync store.
    }
    return nil;
}];
```

The `synchronizeOnConnectivity` method attempts to synchronize when the device has connectivity\. First, `synchronizeOnConnectivity` checks for connectivity and, if the device is online, immediately invokes synchronize and returns the `AWSTask` object associated with the attempt\.

If the device is offline, `synchronizeOnConnectivity` 1\) schedules a synchronize for the next time the device comes online and 2\) returns an `AWSTask` with a nil result\. The scheduled synchronize is only valid for the lifecycle of the dataset object\. The data will not be synchronized if the app is exited before connectivity is regained\. If you want to be notified when events occur during the scheduled synchronize, you must add observers of the notifications found in `AWSCognito`\.

To learn more about dataset synchronization and the different callbacks, see [Handling Callbacks](handling-callbacks.md)\.

### iOS \- Swift<a name="synchronizing-local-data-1.ios-swift"></a>

The `synchronize` method compares local cached data to the data stored in the Amazon Cognito Sync store\. Remote changes are pulled from the Amazon Cognito Sync store; conflict resolution is invoked if any conflicts occur; and updated values on the device are pushed to the service\. To synchronize a dataset, call its `synchronize` method:

The `synchronize` method is asynchronous and returns an `AWSTask` object to handle the response:

```
dataset.synchronize().continueWith(block: { (task) -> AnyObject? in

            if task.isCancelled {
                // Task cancelled.
            } else if task.error != nil {
                // Error while executing task
            } else {
                // Task succeeded. The data was saved in the sync store.
            }
            return task
})
```

The `synchronizeOnConnectivity` method attempts to synchronize when the device has connectivity\. First, `synchronizeOnConnectivity` checks for connectivity and, if the device is online, immediately invokes `synchronize` and returns the `AWSTask` object associated with the attempt\.

If the device is offline, `synchronizeOnConnectivity` 1\) schedules a synchronize for the next time the device comes online and 2\) returns an `AWSTask` object with a nil result\. The scheduled synchronize is only valid for the lifecycle of the dataset object\. The data will not be synchronized if the app is exited before connectivity is regained\. If you want to be notified when events occur during the scheduled synchronize, you must add observers of the notifications found in `AWSCognito`\.

To learn more about dataset synchronization and the different callbacks, see [Handling Callbacks](handling-callbacks.md)\.

### JavaScript<a name="synchronizing-local-data-1.javascript"></a>

The `synchronize` method compares local cached data to the data stored in the Amazon Cognito Sync store\. Remote changes are pulled from the Amazon Cognito Sync store; conflict resolution is invoked if any conflicts occur; and updated values on the device are pushed to the service\. To synchronize a dataset, call its `synchronize` method:

```
dataset.synchronize();
```

To learn more about dataset synchronization and the different callbacks, see [Handling Callbacks](handling-callbacks.md)\.

### Unity<a name="synchronizing-local-data-1.unity"></a>

The synchronize method compares local cached data to the data stored in the Amazon Cognito Sync store\. Remote changes are pulled from the Amazon Cognito Sync store; conflict resolution is invoked if any conflicts occur; and updated values on the device are pushed to the service\. To synchronize a dataset, call its `synchronize` method:

```
dataset.Synchronize();
```

Synchronize will run asynchronously and will end up calling one of the several callbacks you can specify in the Dataset\.

To learn more about dataset synchronization and the different callbacks, see [Handling Callbacks](handling-callbacks.md)\.

### Xamarin<a name="synchronizing-local-data-1.xamarin"></a>

The `synchronize` method compares local cached data to the data stored in the Amazon Cognito Sync store\. Remote changes are pulled from the Amazon Cognito Sync store; conflict resolution is invoked if any conflicts occur; and updated values on the device are pushed to the service\. To synchronize a dataset, call its `synchronize` method:

```
dataset.SynchronizeAsync();
```

To learn more about dataset synchronization and the different callbacks, see [Handling Callbacks](handling-callbacks.md)\.