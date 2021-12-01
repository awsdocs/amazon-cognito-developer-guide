# Handling callbacks<a name="handling-callbacks"></a>

****  
If you're new to Amazon Cognito Sync, use [AWS AppSync](https://aws.amazon.com/appsync/)\. Like Amazon Cognito Sync, AWS AppSync is a service for synchronizing application data across devices\.  
It enables user data like app preferences or game state to be synchronized\. It also extends these capabilities by allowing multiple users to synchronize and collaborate in real time on shared data\.

This section describes how to handle callbacks\.

## Android<a name="handling-callbacks-1.android"></a>

**SyncCallback Interface**

By implementing the `SyncCallback` interface, you can receive notifications on your app about dataset synchronization\. Your app can then make active decisions about deleting local data, merging unauthenticated and authenticated profiles, and resolving sync conflicts\. You should implement the following methods, which are required by the interface:
+ `onSuccess()`
+ `onFailure()`
+ `onConflict()`
+ `onDatasetDeleted()`
+ `onDatasetsMerged()`

Note that, if you don't want to specify all the callbacks, you can also use the class `DefaultSyncCallback` which provides default, empty implementations for all of them\.

**onSuccess**

The `onSuccess()` callback is triggered when a dataset is successfully downloaded from the sync store\.

```
@Override
public void onSuccess(Dataset dataset, List<Record> newRecords) {
}
```

**onFailure**

onFailure\(\) is called if an exception occurs during synchronization\.

```
@Override
public void onFailure(DataStorageException dse) {
}
```

**onConflict**

Conflicts may arise if the same key has been modified on the local store and in the sync store\. The `onConflict()` method handles conflict resolution\. If you don't implement this method, the Amazon Cognito Sync client defaults to using the most recent change\.

```
@Override
public boolean onConflict(Dataset dataset, final List<SyncConflict> conflicts) {
    List<Record> resolvedRecords = new ArrayList<Record>();
    for (SyncConflict conflict : conflicts) {
        /* resolved by taking remote records */
        resolvedRecords.add(conflict.resolveWithRemoteRecord());

        /* alternately take the local records */
        // resolvedRecords.add(conflict.resolveWithLocalRecord());

        /* or customer logic, say concatenate strings */
        // String newValue = conflict.getRemoteRecord().getValue()
        //     + conflict.getLocalRecord().getValue();
        // resolvedRecords.add(conflict.resolveWithValue(newValue);
    }
    dataset.resolve(resolvedRecords);

    // return true so that synchronize() is retried after conflicts are resolved
    return true;
}
```

**onDatasetDeleted**

When a dataset is deleted, the Amazon Cognito client uses the `SyncCallback` interface to confirm whether the local cached copy of the dataset should be deleted too\. Implement the `onDatasetDeleted()` method to tell the client SDK what to do with the local data\.

```
@Override
public boolean onDatasetDeleted(Dataset dataset, String datasetName) {
    // return true to delete the local copy of the dataset
    return true;
}
```

**onDatasetMerged**

When two previously unconnected identities are linked together, all of their datasets are merged\. Applications are notified of the merge through the `onDatasetsMerged()` method:

```
@Override
public boolean onDatasetsMerged(Dataset dataset, List<String> datasetNames) {
    // return false to handle Dataset merge outside the synchronization callback
    return false;
}
```

## iOS \- Objective\-C<a name="handling-callbacks-1.ios-objc"></a>

**Sync Notifications**

The Amazon Cognito client will emit a number of `NSNotification` events during a synchronize call\. You can register to monitor these notifications via the standard `NSNotificationCenter`:

```
[NSNotificationCenter defaultCenter]
  addObserver:self
  selector:@selector(myNotificationHandler:)
  name:NOTIFICATION_TYPE
  object:nil];
```

Amazon Cognito supports five notification types, listed below\.

**AWSCognitoDidStartSynchronizeNotification**

Called when a synchronize operation is starting\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized\.

**AWSCognitoDidEndSynchronizeNotification**

Called when a synchronize operation completes \(successfully or otherwise\)\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized\.

**AWSCognitoDidFailToSynchronizeNotification**

Called when a synchronize operation fails\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized and the key error which will contain the error that caused the failure\.

**AWSCognitoDidChangeRemoteValueNotification**

Called when local changes are successfully pushed to Amazon Cognito\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized and the key keys which will contain an NSArray of record keys that were pushed\.

**AWSCognitoDidChangeLocalValueFromRemoteNotification**

Called when a local value changes due to a synchronize operation\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized and the key keys which will contain an NSArray of record keys that changed\.

**Conflict Resolution Handler**

During a sync operation, conflicts may arise if the same key has been modified on the local store and in the sync store\. If you haven't set a conflict resolution handler, Amazon Cognito defaults to choosing the most recent update\.

By implementing and assigning an AWSCognitoRecordConflictHandler you can alter the default conflict resolution\. The AWSCognitoConflict input parameter conflict contains an AWSCognitoRecord object for both the local cached data and for the conflicting record in the sync store\. Using the AWSCognitoConflict you can resolve the conflict with the local record: \[conflict resolveWithLocalRecord\], the remote record: \[conflict resolveWithRemoteRecord\] or a brand new value: \[conflict resolveWithValue:value\]\. Returning nil from this method prevents synchronization from continuing and the conflicts will be presented again the next time the sync process starts\.

You can set the conflict resolution handler at the client level:

```
client.conflictHandler = ^AWSCognitoResolvedConflict* (NSString *datasetName, AWSCognitoConflict *conflict) {
    // always choose local changes
    return [conflict resolveWithLocalRecord];
};
```

Or at the dataset level:

```
dataset.conflictHandler = ^AWSCognitoResolvedConflict* (NSString *datasetName, AWSCognitoConflict *conflict) {
    // override and always choose remote changes
    return [conflict resolveWithRemoteRecord];
};
```

**Dataset Deleted Handler**

When a dataset is deleted, the Amazon Cognito client uses the `AWSCognitoDatasetDeletedHandler` to confirm whether the local cached copy of the dataset should be deleted too\. If no `AWSCognitoDatasetDeletedHandler` is implemented, the local data will be purged automatically\. Implement an `AWSCognitoDatasetDeletedHandler` if you wish to keep a copy of the local data before wiping, or to keep the local data\.

You can set the dataset deleted handler at the client level:

```
client.datasetDeletedHandler = ^BOOL (NSString *datasetName) {
    // make a backup of the data if you choose
    ...
    // delete the local data (default behavior)
    return YES;
};
```

Or at the dataset level:

```
dataset.datasetDeletedHandler = ^BOOL (NSString *datasetName) {
    // override default and keep the local data
    return NO;
};
```

**Dataset Merge Handler**

When two previously unconnected identities are linked together, all of their datasets are merged\. Applications are notified of the merge through the `DatasetMergeHandler`\. The handler will receive the name of the root dataset as well as an array of dataset names that are marked as merges of the root dataset\.

If no `DatasetMergeHandler` is implemented, these datasets will be ignored, but will continue to use up space in the identity's 20 maximum total datasets\.

You can set the dataset merge handler at the client level:

```
client.datasetMergedHandler = ^(NSString *datasetName, NSArray *datasets) {
    // Blindly delete the datasets
    for (NSString *name in datasets) {
        AWSCognitoDataset *merged = [[AWSCognito defaultCognito] openOrCreateDataset:name];
       [merged clear];
       [merged synchronize];
    }
};
```

Or at the dataset level:

```
dataset.datasetMergedHandler = ^(NSString *datasetName, NSArray *datasets) {
    // Blindly delete the datasets
    for (NSString *name in datasets) {
        AWSCognitoDataset *merged = [[AWSCognito defaultCognito] openOrCreateDataset:name];
        // do something with the data if it differs from existing dataset
        ...
        // now delete it
        [merged clear];
        [merged synchronize];
    }
};
```

## iOS \- Swift<a name="handling-callbacks-1.ios-swift"></a>

**Sync Notifications**

The Amazon Cognito client will emit a number of `NSNotification` events during a synchronize call\. You can register to monitor these notifications via the standard `NSNotificationCenter`:

```
NSNotificationCenter.defaultCenter().addObserver(observer: self,
   selector: "myNotificationHandler",
   name:NOTIFICATION_TYPE,
   object:nil)
```

Amazon Cognito supports five notification types, listed below\.

**AWSCognitoDidStartSynchronizeNotification**

Called when a synchronize operation is starting\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized\.

**AWSCognitoDidEndSynchronizeNotification**

Called when a synchronize operation completes \(successfully or otherwise\)\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized\.

**AWSCognitoDidFailToSynchronizeNotification**

Called when a synchronize operation fails\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized and the key error which will contain the error that caused the failure\.

**AWSCognitoDidChangeRemoteValueNotification**

Called when local changes are successfully pushed to Amazon Cognito\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized and the key keys which will contain an NSArray of record keys that were pushed\.

**AWSCognitoDidChangeLocalValueFromRemoteNotification**

Called when a local value changes due to a synchronize operation\. The `userInfo` will contain the key dataset which is the name of the dataset being synchronized and the key keys which will contain an NSArray of record keys that changed\.

**Conflict Resolution Handler**

During a sync operation, conflicts may arise if the same key has been modified on the local store and in the sync store\. If you haven't set a conflict resolution handler, Amazon Cognito defaults to choosing the most recent update\.

By implementing and assigning an `AWSCognitoRecordConflictHandler` you can alter the default conflict resolution\. The `AWSCognitoConflict` input parameter conflict contains an `AWSCognitoRecord` object for both the local cached data and for the conflicting record in the sync store\. Using the `AWSCognitoConflict` you can resolve the conflict with the local record: \[conflict resolveWithLocalRecord\], the remote record: \[conflict resolveWithRemoteRecord\] or a brand new value: \[conflict resolveWithValue:value\]\. Returning nil from this method prevents synchronization from continuing and the conflicts will be presented again the next time the sync process starts\.

You can set the conflict resolution handler at the client level:

```
client.conflictHandler = {
    (datasetName: String?, conflict: AWSCognitoConflict?) -> AWSCognitoResolvedConflict? in
    return conflict.resolveWithLocalRecord()
}
```

Or at the dataset level:

```
dataset.conflictHandler = {
    (datasetName: String?, conflict: AWSCognitoConflict?) -> AWSCognitoResolvedConflict? in
    return conflict.resolveWithLocalRecord()
}
```

**Dataset Deleted Handler**

When a dataset is deleted, the Amazon Cognito client uses the `AWSCognitoDatasetDeletedHandler` to confirm whether the local cached copy of the dataset should be deleted too\. If no `AWSCognitoDatasetDeletedHandler` is implemented, the local data will be purged automatically\. Implement an `AWSCognitoDatasetDeletedHandler` if you wish to keep a copy of the local data before wiping, or to keep the local data\.

You can set the dataset deleted handler at the client level:

```
client.datasetDeletedHandler = {
      (datasetName: String!) -> Bool in
      // make a backup of the data if you choose
      ...
      // delete the local data (default behaviour)
      return true
}
```

Or at the dataset level:

```
dataset.datasetDeletedHandler = {
      (datasetName: String!) -> Bool in
      // make a backup of the data if you choose
      ...
      // delete the local data (default behaviour)
      return true
}
```

**Dataset merge handler**

When two previously unconnected identities are linked together, all of their datasets are merged\. Applications are notified of the merge through the `DatasetMergeHandler`\. The handler will receive the name of the root dataset as well as an array of dataset names that are marked as merges of the root dataset\.

If no `DatasetMergeHandler` is implemented, these datasets will be ignored, but will continue to use up space in the identity's 20 maximum total datasets\.

You can set the dataset merge handler at the client level:

```
client.datasetMergedHandler = {
    (datasetName: String!, datasets: [AnyObject]!) -> Void in
    for nameObject in datasets {
        if let name = nameObject as? String {
            let merged = AWSCognito.defaultCognito().openOrCreateDataset(name)
            merged.clear()
            merged.synchronize()
        }
    }
}
```

Or at the dataset level:

```
dataset.datasetMergedHandler = {
    (datasetName: String!, datasets: [AnyObject]!) -> Void in
    for nameObject in datasets {
        if let name = nameObject as? String {
            let merged = AWSCognito.defaultCognito().openOrCreateDataset(name)
            // do something with the data if it differs from existing dataset
              ...
              // now delete it
            merged.clear()
            merged.synchronize()
        }
    }
}
```

## JavaScript<a name="handling-callbacks-1.javascript"></a>

**Synchronization callbacks**

When performing a synchronize\(\) on a dataset, you can optionally specify callbacks to handle each of the following states:

```
dataset.synchronize({

   onSuccess: function(dataset, newRecords) {
      //...
   },

   onFailure: function(err) {
      //...
   },

   onConflict: function(dataset, conflicts, callback) {
      //...
   },

   onDatasetDeleted: function(dataset, datasetName, callback) {
      //...
   },

  onDatasetMerged: function(dataset, datasetNames, callback) {
      //...
  }

});
```

**onSuccess\(\)**

The `onSuccess()` callback is triggered when a dataset is successfully updated from the sync store\. If you do not define a callback, the synchronization will succeed silently\.

```
onSuccess: function(dataset, newRecords) {
   console.log('Successfully synchronized ' + newRecords.length + ' new records.');
}
```

**onFailure\(\)**

`onFailure()` is called if an exception occurs during synchronization\. If you do not define a callback, the synchronization will fail silently\.

```
onFailure: function(err) {
   console.log('Synchronization failed.');
   console.log(err);
}
```

**onConflict\(\)**

Conflicts may arise if the same key has been modified on the local store and in the sync store\. The `onConflict()` method handles conflict resolution\. If you don't implement this method, the synchronization will be aborted when there is a conflict\.

```
onConflict: function(dataset, conflicts, callback) {

   var resolved = [];

   for (var i=0; i<conflicts.length; i++) {

      // Take remote version.
      resolved.push(conflicts[i].resolveWithRemoteRecord());

      // Or... take local version.
      // resolved.push(conflicts[i].resolveWithLocalRecord());

      // Or... use custom logic.
      // var newValue = conflicts[i].getRemoteRecord().getValue() + conflicts[i].getLocalRecord().getValue();
      // resolved.push(conflicts[i].resovleWithValue(newValue);

   }

   dataset.resolve(resolved, function() {
      return callback(true);
   });

   // Or... callback false to stop the synchronization process.
   // return callback(false);

}
```

**onDatasetDeleted\(\)**

When a dataset is deleted, the Amazon Cognito client uses the `onDatasetDeleted()` callback to decide whether the local cached copy of the dataset should be deleted too\. By default, the dataset will not be deleted\.

```
onDatasetDeleted: function(dataset, datasetName, callback) {

   // Return true to delete the local copy of the dataset.
   // Return false to handle deleted datasets outside the synchronization callback.

   return callback(true);

}
```

**onDatasetMerged\(\)**

When two previously unconnected identities are linked together, all of their datasets are merged\. Applications are notified of the merge through the `onDatasetsMerged()` callback\.

```
onDatasetMerged: function(dataset, datasetNames, callback) {

   // Return true to continue the synchronization process.
   // Return false to handle dataset merges outside the synchronization callback.

   return callback(false);

}
```

## Unity<a name="handling-callbacks-1.unity"></a>

After you open or create a dataset, you can set different callbacks to it that will be triggered when you use the Synchronize method\. This is the way to register your callbacks to them:

```
dataset.OnSyncSuccess += this.HandleSyncSuccess;
dataset.OnSyncFailure += this.HandleSyncFailure;
dataset.OnSyncConflict = this.HandleSyncConflict;
dataset.OnDatasetMerged = this.HandleDatasetMerged;
dataset.OnDatasetDeleted = this.HandleDatasetDeleted;
```

Note that `SyncSuccess` and `SyncFailure` use \+= instead of = so you can subscribe more than one callback to them\.

**OnSyncSuccess**

The `OnSyncSuccess` callback is triggered when a dataset is successfully updated from the cloud\. If you do not define a callback, the synchronization will succeed silently\.

```
private void HandleSyncSuccess(object sender, SyncSuccessEvent e)
{
    // Continue with your game flow, display the loaded data, etc.
}
```

**OnSyncFailure**

`OnSyncFailure` is called if an exception occurs during synchronization\. If you do not define a callback, the synchronization will fail silently\.

```
private void HandleSyncFailure(object sender, SyncFailureEvent e)
{
    Dataset dataset = sender as Dataset;
    if (dataset.Metadata != null) {
        Debug.Log("Sync failed for dataset : " + dataset.Metadata.DatasetName);
    } else {
        Debug.Log("Sync failed");
    }
    // Handle the error
    Debug.LogException(e.Exception);
}
```

**OnSyncConflict**

Conflicts may arise if the same key has been modified on the local store and in the sync store\. The `OnSyncConflict` callback handles conflict resolution\. If you don't implement this method, the synchronization will be aborted when there is a conflict\.

```
private bool HandleSyncConflict(Dataset dataset, List < SyncConflict > conflicts)
{
  if (dataset.Metadata != null) {
    Debug.LogWarning("Sync conflict " + dataset.Metadata.DatasetName);
  } else {
    Debug.LogWarning("Sync conflict");
  }
  List < Amazon.CognitoSync.SyncManager.Record > resolvedRecords = new List < Amazon.CognitoSync.SyncManager.Record > ();
  foreach(SyncConflict conflictRecord in conflicts) {
    // SyncManager provides the following default conflict resolution methods:
    //      ResolveWithRemoteRecord - overwrites the local with remote records
    //      ResolveWithLocalRecord - overwrites the remote with local records
    //      ResolveWithValue - to implement your own logic
    resolvedRecords.Add(conflictRecord.ResolveWithRemoteRecord());
  }
  // resolves the conflicts in local storage
  dataset.Resolve(resolvedRecords);
  // on return true the synchronize operation continues where it left,
  //      returning false cancels the synchronize operation
  return true;
}
```

**OnDatasetDeleted**

When a dataset is deleted, the Amazon Cognito client uses the `OnDatasetDeleted` callback to decide whether the local cached copy of the dataset should be deleted too\. By default, the dataset will not be deleted\.

```
private bool HandleDatasetDeleted(Dataset dataset)
  {
      Debug.Log(dataset.Metadata.DatasetName + " Dataset has been deleted");
      // Do clean up if necessary
      // returning true informs the corresponding dataset can be purged in the local storage and return false retains the local dataset
      return true;
  }
```

**OnDatasetMerged**

When two previously unconnected identities are linked together, all of their datasets are merged\. Applications are notified of the merge through the `OnDatasetsMerged` callback\.

```
public bool HandleDatasetMerged(Dataset localDataset, List<string> mergedDatasetNames)
{
    foreach (string name in mergedDatasetNames)
    {
        Dataset mergedDataset = syncManager.OpenOrCreateDataset(name);
        //Lambda function to delete the dataset after fetching it
        EventHandler<SyncSuccessEvent> lambda;
        lambda = (object sender, SyncSuccessEvent e) => {
            ICollection<string> existingValues = localDataset.GetAll().Values;
            ICollection<string> newValues = mergedDataset.GetAll().Values;

            //Implement your merge logic here

            mergedDataset.Delete(); //Delete the dataset locally
            mergedDataset.OnSyncSuccess -= lambda; //We don't want this callback to be fired again
            mergedDataset.OnSyncSuccess += (object s2, SyncSuccessEvent e2) => {
                localDataset.Synchronize(); //Continue the sync operation that was interrupted by the merge
            };
            mergedDataset.Synchronize(); //Synchronize it as deleted, failing to do so will leave us in an inconsistent state
        };
        mergedDataset.OnSyncSuccess += lambda;
        mergedDataset.Synchronize(); //Asnchronously fetch the dataset
    }

    // returning true allows the Synchronize to continue and false stops it
    return false;
}
```

## Xamarin<a name="handling-callbacks-1.xamarin"></a>

After you open or create a dataset, you can set different callbacks to it that will be triggered when you use the Synchronize method\. This is the way to register your callbacks to them:

```
dataset.OnSyncSuccess += this.HandleSyncSuccess;
dataset.OnSyncFailure += this.HandleSyncFailure;
dataset.OnSyncConflict = this.HandleSyncConflict;
dataset.OnDatasetMerged = this.HandleDatasetMerged;
dataset.OnDatasetDeleted = this.HandleDatasetDeleted;
```

Note that `SyncSuccess` and `SyncFailure` use \+= instead of = so you can subscribe more than one callback to them\.

**OnSyncSuccess**

The `OnSyncSuccess` callback is triggered when a dataset is successfully updated from the cloud\. If you do not define a callback, the synchronization will succeed silently\.

```
private void HandleSyncSuccess(object sender, SyncSuccessEventArgs e)
{
    // Continue with your game flow, display the loaded data, etc.
}
```

**OnSyncFailure**

`OnSyncFailure` is called if an exception occurs during synchronization\. If you do not define a callback, the synchronization will fail silently\.

```
private void HandleSyncFailure(object sender, SyncFailureEventArgs e)
{
    Dataset dataset = sender as Dataset;
    if (dataset.Metadata != null) {
        Console.WriteLine("Sync failed for dataset : " + dataset.Metadata.DatasetName);
    } else {
        Console.WriteLine("Sync failed");
    }
}
```

**OnSyncConflict**

Conflicts may arise if the same key has been modified on the local store and in the sync store\. The `OnSyncConflict` callback handles conflict resolution\. If you don't implement this method, the synchronization will be aborted when there is a conflict\.

```
private bool HandleSyncConflict(Dataset dataset, List < SyncConflict > conflicts)
{
  if (dataset.Metadata != null) {
    Console.WriteLine("Sync conflict " + dataset.Metadata.DatasetName);
  } else {
    Console.WriteLine("Sync conflict");
  }
  List < Amazon.CognitoSync.SyncManager.Record > resolvedRecords = new List < Amazon.CognitoSync.SyncManager.Record > ();
  foreach(SyncConflict conflictRecord in conflicts) {
    // SyncManager provides the following default conflict resolution methods:
    //      ResolveWithRemoteRecord - overwrites the local with remote records
    //      ResolveWithLocalRecord - overwrites the remote with local records
    //      ResolveWithValue - to implement your own logic
    resolvedRecords.Add(conflictRecord.ResolveWithRemoteRecord());
  }
  // resolves the conflicts in local storage
  dataset.Resolve(resolvedRecords);
  // on return true the synchronize operation continues where it left,
  //      returning false cancels the synchronize operation
  return true;
}
```

**OnDatasetDeleted**

When a dataset is deleted, the Amazon Cognito client uses the `OnDatasetDeleted` callback to decide whether the local cached copy of the dataset should be deleted too\. By default, the dataset will not be deleted\.

```
private bool HandleDatasetDeleted(Dataset dataset)
{
    Console.WriteLine(dataset.Metadata.DatasetName + " Dataset has been deleted");
    // Do clean up if necessary
    // returning true informs the corresponding dataset can be purged in the local storage and return false retains the local dataset
    return true;
}
```

**OnDatasetMerged**

When two previously unconnected identities are linked together, all of their datasets are merged\. Applications are notified of the merge through the `OnDatasetsMerged` callback\.

```
public bool HandleDatasetMerged(Dataset localDataset, List<string> mergedDatasetNames)
{
    foreach (string name in mergedDatasetNames)
    {
        Dataset mergedDataset = syncManager.OpenOrCreateDataset(name);

            //Implement your merge logic here

        mergedDataset.OnSyncSuccess += lambda;
        mergedDataset.SynchronizeAsync(); //Asnchronously fetch the dataset
    }

    // returning true allows the Synchronize to continue and false stops it
    return false;
}
```