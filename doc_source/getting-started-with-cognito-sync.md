# Getting started with Amazon Cognito Sync<a name="getting-started-with-cognito-sync"></a>

****  
If you're new to Amazon Cognito Sync, use [AWS AppSync](https://aws.amazon.com/appsync/)\. Like Amazon Cognito Sync, AWS AppSync is a service for synchronizing application data across devices\.  
It enables user data like app preferences or game state to be synchronized\. It also extends these capabilities by allowing multiple users to synchronize and collaborate in real time on shared data\.

Amazon Cognito Sync is an AWS service and client library that enable cross\-device syncing of application\-related user data\. You can use it to synchronize user profile data across mobile devices and web applications\. The client libraries cache data locally so your app can read and write data regardless of device connectivity status\. When the device is online, you can synchronize data, and if you set up push sync, notify other devices immediately that an update is available\.

## Sign up for an AWS account<a name="aws-sign-up-sync"></a>

To use Amazon Cognito Sync, you need an AWS account\. If you don't already have one, use the following procedure to sign up: 

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Set up an identity pool in Amazon Cognito<a name="set-up-an-identity-pool"></a>

Amazon Cognito Sync requires an Amazon Cognito identity pool to provide user identities\. Thus you need to first set up an identity pool before using Amazon Cognito Sync\. Follow the [Getting started with Amazon Cognito identity pools \(federated identities\)](getting-started-with-identity-pools.md) guide to create an identity pool and install the SDK\.

## Store and sync data<a name="store-and-sync-data"></a>

Once you have set up your identity pool and installed the SDK, you can start storing and syncing data between devices\. See [Synchronizing data](synchronizing-data.md) for more information\.