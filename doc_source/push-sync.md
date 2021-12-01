# Push sync<a name="push-sync"></a>

****  
If you're new to Amazon Cognito Sync, use [AWS AppSync](https://aws.amazon.com/appsync/)\. Like Amazon Cognito Sync, AWS AppSync is a service for synchronizing application data across devices\.  
It enables user data like app preferences or game state to be synchronized\. It also extends these capabilities by allowing multiple users to synchronize and collaborate in real time on shared data\.

Amazon Cognito automatically tracks the association between identity and devices\. Using the push synchronization, or push sync, feature, you can ensure that every instance of a given identity is notified when identity data changes\. Push sync ensures that, whenever the sync store data changes for a particular identity, all devices associated with that identity receive a silent push notification informing them of the change\.

**Note**  
Push sync is not supported for JavaScript, Unity, or Xamarin\.

Before you can use push sync, you must first set up your account for push sync and enable push sync in the Amazon Cognito console\.

## Create an Amazon Simple Notification Service \(Amazon SNS\) app<a name="create-an-amazon-SNS-app"></a>

Create and configure an Amazon SNS app for your supported platforms, as described in the [SNS Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/SNSMobilePush.html)\.

## Enable push sync in the Amazon Cognito console<a name="enable-push-sync-in-the-amazon-cognito-console"></a>

You can enable push sync via the Amazon Cognito console\. From the [console home page](https://console.aws.amazon.com/cognito/home):

1. Click the name of the identity pool for which you want to enable push sync\. The **Dashboard** page for your identity pool appears\.

1. In the top\-right corner of the **Dashboard** page, click **Manage Identity Pools**\. The **Federated Identities** page appears\.

1. Scroll down and click **Push synchronization** to expand it\.

1. In the **Service role** dropdown menu, select the IAM role that grants Cognito permission to send an SNS notification\. Click **Create role** to create or modify the roles associated with your identity pool in the [AWS IAM Console](https://console.aws.amazon.com/iam/home)\.

1. Select a platform application, and then click **Save Changes**\.

1. Grant SNS Access to Your Application

In the AWS Identity and Access Management console, configure your IAM roles to have full Amazon SNS access, or create a new role that has full Amazon SNS access\. The following example role trust policy grants Amazon Cognito Sync a limited ability to assume an IAM role\. Amazon Cognito Sync can only assume the role when it does so on behalf of both the identity pool in the `aws:SourceArn` condition and the account in the `aws:SourceAccount` condition\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "cognito-sync.amazonaws.com"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceAccount": "123456789012"
                },
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cognito-identity:us-east-1:123456789012:identitypool/us-east-1:177a950c-2c08-43f0-9983-28727EXAMPLE"
                }
            }
        }
    ]
}
```

To learn more about IAM roles, see [Roles \(Delegation and Federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/WorkingWithRoles.html)\.

## Use push sync in your app: Android<a name="push-sync-1.android"></a>

Your application will need to import the Google Play services\. You can download the latest version of the Google Play SDK via the [Android SDK manager](http://developer.android.com/tools/help/sdk-manager.html)\. Follow the Android documentation on [Android Implementation](https://developers.google.com/instance-id/guides/android-implementation) to register your app and receive a registration ID from GCM\. Once you have the registration ID, you need to register the device with Amazon Cognito, as shown in the snippet below:

```
String registrationId = "MY_GCM_REGISTRATION_ID";
try {
    client.registerDevice("GCM", registrationId);
} catch (RegistrationFailedException rfe) {
    Log.e(TAG, "Failed to register device for silent sync", rfe);
} catch (AmazonClientException ace) {
    Log.e(TAG, "An unknown error caused registration for silent sync to fail", ace);
}
```

You can now subscribe a device to receive updates from a particular dataset:

```
Dataset trackedDataset = client.openOrCreateDataset("myDataset");
if (client.isDeviceRegistered()) {
    try {
        trackedDataset.subscribe();
    } catch (SubscribeFailedException sfe) {
        Log.e(TAG, "Failed to subscribe to datasets", sfe);
    } catch (AmazonClientException ace) {
        Log.e(TAG, "An unknown error caused the subscription to fail", ace);
    }
}
```

To stop receiving push notifications from a dataset, simply call the unsubscribe method\. To subscribe to all datasets \(or a specific subset\) in the `CognitoSyncManager` object, use `subscribeAll()`:

```
if (client.isDeviceRegistered()) {
    try {
        client.subscribeAll();
    } catch (SubscribeFailedException sfe) {
        Log.e(TAG, "Failed to subscribe to datasets", sfe);
    } catch (AmazonClientException ace) {
        Log.e(TAG, "An unknown error caused the subscription to fail", ace);
    }
}
```

In your implementation of the [Android BroadcastReceiver](http://developer.android.com/reference/android/content/BroadcastReceiver.html) object, you can check the latest version of the modified dataset and decide if your app needs to synchronize again:

```
@Override
public void onReceive(Context context, Intent intent) {

    PushSyncUpdate update = client.getPushSyncUpdate(intent);

    // The update has the source (cognito-sync here), identityId of the
    // user, identityPoolId in question, the non-local sync count of the
    // data set and the name of the dataset. All are accessible through
    // relevant getters.

    String source = update.getSource();
    String identityPoolId = update.getIdentityPoolId();
    String identityId = update.getIdentityId();
    String datasetName = update.getDatasetName;
    long syncCount = update.getSyncCount;

    Dataset dataset = client.openOrCreateDataset(datasetName);

    // need to access last sync count. If sync count is less or equal to
    // last sync count of the dataset, no sync is required.

    long lastSyncCount = dataset.getLastSyncCount();
    if (lastSyncCount < syncCount) {
        dataset.synchronize(new SyncCallback() {
            // ...
        });
    }

}
```

The following keys are available in the push notification payload:
+ `source`: cognito\-sync\. This can serve as a differentiating factor between notifications\.
+ `identityPoolId`: The identity pool ID\. This can be used for validation or additional information, though it's not integral from the receiver's point of view\.
+ `identityId`: The identity ID within the pool\.
+ `datasetName`: The name of the dataset that was updated\. This is available for the sake of the openOrCreateDataset call\.
+ `syncCount`: The sync count for the remote dataset\. You can use this as a way to make sure that the local dataset is out of date, and that the incoming synchronization is new\.

## Use push sync in your app: iOS \- Objective\-C<a name="push-sync-1.ios-objc"></a>

To obtain a device token for your app, follow the Apple documentation on Registering for Remote Notifications\. Once you've received the device token as an NSData object from APNs, you'll need to register the device with Amazon Cognito using the `registerDevice:` method of the sync client, as shown below:

```
AWSCognito *syncClient = [AWSCognito defaultCognito];
    [[syncClient registerDevice: devToken] continueWithBlock:^id(AWSTask *task) {
        if(task.error){
            NSLog(@"Unable to registerDevice: %@", task.error);
        } else {
            NSLog(@"Successfully registered device with id: %@", task.result);
        }
        return nil;
      }
    ];
```

In debug mode, your device will register with the APNs sandbox; in release mode, it will register with APNs\. To receive updates from a particular dataset, use the `subscribe` method:

```
[[[syncClient openOrCreateDataset:@"MyDataset"] subscribe] continueWithBlock:^id(AWSTask *task) {
        if(task.error){
            NSLog(@"Unable to subscribe to dataset: %@", task.error);
        } else {
            NSLog(@"Successfully subscribed to dataset: %@", task.result);
        }
        return nil;
      }
    ];
```

To stop receiving push notifications from a dataset, simply call the `unsubscribe` method:

```
[[[syncClient openOrCreateDataset:@”MyDataset”] unsubscribe] continueWithBlock:^id(AWSTask *task) {
        if(task.error){
            NSLog(@"Unable to unsubscribe from dataset: %@", task.error);
        } else {
            NSLog(@"Successfully unsubscribed from dataset: %@", task.result);
        }
        return nil;
      }
    ];
```

To subscribe to all datasets in the `AWSCognito` object, call `subscribeAll`:

```
[[syncClient subscribeAll] continueWithBlock:^id(AWSTask *task) {
        if(task.error){
            NSLog(@"Unable to subscribe to all datasets: %@", task.error);
        } else {
            NSLog(@"Successfully subscribed to all datasets: %@", task.result);
        }
        return nil;
      }
    ];
```

Before calling `subscribeAll`, be sure to synchronize at least once on each dataset, so that the datasets exist on the server\.

To react to push notifications, you need to implement the `didReceiveRemoteNotification` method in your app delegate:

```
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
    {
        [[NSNotificationCenter defaultCenter] postNotificationName:@"CognitoPushNotification" object:userInfo];
    }
```

If you post a notification using notification handler, you can then respond to the notification elsewhere in the application where you have a handle to your dataset\. If you subscribe to the notification like this \.\.\.

```
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didReceivePushSync:)
    name: :@"CognitoPushNotification" object:nil];
```

\.\.\.you can act on the notification like this:

```
- (void)didReceivePushSync:(NSNotification*)notification
    {
        NSDictionary * data = [(NSDictionary *)[notification object] objectForKey:@"data"];
        NSString * identityId = [data objectForKey:@"identityId"];
        NSString * datasetName = [data objectForKey:@"datasetName"];
        if([self.dataset.name isEqualToString:datasetName] && [self.identityId isEqualToString:identityId]){
            [[self.dataset synchronize] continueWithBlock:^id(AWSTask *task) {
                if(!task.error){
                    NSLog(@"Successfully synced dataset");
                }
                return nil;
            }];
        }
    }
```

The following keys are available in the push notification payload:
+ `source`: cognito\-sync\. This can serve as a differentiating factor between notifications\.
+ `identityPoolId`: The identity pool ID\. This can be used for validation or additional information, though it's not integral from the receiver's point of view\.
+ `identityId`: The identity ID within the pool\.
+ `datasetName`: The name of the dataset that was updated\. This is available for the sake of the `openOrCreateDataset` call\.
+ `syncCount`: The sync count for the remote dataset\. You can use this as a way to make sure that the local dataset is out of date, and that the incoming synchronization is new\.

## Use push sync in your app: iOS \- Swift<a name="push-sync-1.ios-swift"></a>

To obtain a device token for your app, follow the Apple documentation on Registering for Remote Notifications\. Once you've received the device token as an NSData object from APNs, you'll need to register the device with Amazon Cognito using the registerDevice: method of the sync client, as shown below:

```
let syncClient = AWSCognito.default()
syncClient.registerDevice(devToken).continueWith(block: { (task: AWSTask!) -> AnyObject! in
    if (task.error != nil) {
        print("Unable to register device: " + task.error.localizedDescription)

    } else {
        print("Successfully registered device with id: \(task.result)")
    }
    return task
})
```

In debug mode, your device will register with the APNs sandbox; in release mode, it will register with APNs\. To receive updates from a particular dataset, use the `subscribe` method:

```
syncClient.openOrCreateDataset("MyDataset").subscribe().continueWith(block: { (task: AWSTask!) -> AnyObject! in
  if (task.error != nil) {
      print("Unable to subscribe to dataset: " + task.error.localizedDescription)

  } else {
      print("Successfully subscribed to dataset: \(task.result)")
  }
  return task
})
```

To stop receiving push notifications from a dataset, call the `unsubscribe` method:

```
syncClient.openOrCreateDataset("MyDataset").unsubscribe().continueWith(block: { (task: AWSTask!) -> AnyObject! in
  if (task.error != nil) {
      print("Unable to unsubscribe to dataset: " + task.error.localizedDescription)

  } else {
      print("Successfully unsubscribed to dataset: \(task.result)")
  }
  return task
})
```

To subscribe to all datasets in the `AWSCognito` object, call `subscribeAll`:

```
syncClient.openOrCreateDataset("MyDataset").subscribeAll().continueWith(block: { (task: AWSTask!) -> AnyObject! in
  if (task.error != nil) {
      print("Unable to subscribe to all datasets: " + task.error.localizedDescription)

  } else {
      print("Successfully subscribed to all datasets: \(task.result)")
  }
  return task
})
```

Before calling `subscribeAll`, be sure to synchronize at least once on each dataset, so that the datasets exist on the server\.

To react to push notifications, you need to implement the `didReceiveRemoteNotification` method in your app delegate:

```
func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject],
  fetchCompletionHandler completionHandler: (UIBackgroundFetchResult) -> Void) {
        NSNotificationCenter.defaultCenter().postNotificationName("CognitoPushNotification", object: userInfo)
})
```

If you post a notification using notification handler, you can then respond to the notification elsewhere in the application where you have a handle to your dataset\. If you subscribe to the notification like this \.\.\.

```
NSNotificationCenter.defaultCenter().addObserver(observer:self,
   selector:"didReceivePushSync:",
   name:"CognitoPushNotification",
   object:nil)
```

\.\.\.you can act on the notification like this:

```
func didReceivePushSync(notification: NSNotification) {
    if let data = (notification.object as! [String: AnyObject])["data"] as? [String: AnyObject] {
        let identityId = data["identityId"] as! String
        let datasetName = data["datasetName"] as! String

        if self.dataset.name == datasetName && self.identityId == identityId {
          dataset.synchronize().continueWithBlock {(task) -> AnyObject! in
              if task.error == nil {
                print("Successfully synced dataset")
              }
              return nil
          }
        }
    }
}
```

The following keys are available in the push notification payload:
+ `source`: cognito\-sync\. This can serve as a differentiating factor between notifications\.
+ `identityPoolId`: The identity pool ID\. This can be used for validation or additional information, though it's not integral from the receiver's point of view\.
+ `identityId`: The identity ID within the pool\.
+ `datasetName`: The name of the dataset that was updated\. This is available for the sake of the `openOrCreateDataset` call\.
+ `syncCount`: The sync count for the remote dataset\. You can use this as a way to make sure that the local dataset is out of date, and that the incoming synchronization is new\.