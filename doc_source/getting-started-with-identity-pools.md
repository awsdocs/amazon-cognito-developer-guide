# Getting Started with Amazon Cognito Identity Pools \(Federated Identities\)<a name="getting-started-with-identity-pools"></a>

Amazon Cognito identity pools enable you to create unique identities and assign permissions for users\. Your identity pool can include: 
+ Users in an Amazon Cognito user pool
+ Users who authenticate with external identity providers such as Facebook, Google, Apple, or a SAML\-based identity provider
+ Users authenticated via your own existing authentication process

 With an identity pool, you can obtain temporary AWS credentials with permissions you define to directly access other AWS services or to access resources through Amazon API Gateway\.

**Topics**
+ [Sign Up for an AWS Account](#aws-sign-up-identity-pools)
+ [Create an Identity Pool in Amazon Cognito](#create-identity-pool)
+ [Install the Mobile or JavaScript SDK](#install-the-mobile-or-javascript-sdk)
+ [Integrate the Identity Providers](#integrate-the-identity-providers)
+ [Get Credentials](#get-credentials)

## Sign Up for an AWS Account<a name="aws-sign-up-identity-pools"></a>

To use Amazon Cognito identity pools, you need an AWS account\. If you don't already have one, use the following procedure to sign up: 

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Create an Identity Pool in Amazon Cognito<a name="create-identity-pool"></a>

You can create an identity pool through the Amazon Cognito console, or you can use the AWS Command Line Interface \(CLI\) or the Amazon Cognito APIs\.

**To create a new identity pool in the console**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), choose **Manage Identity Pools**, and then choose **Create new identity pool**\.

1. Type a name for your identity pool\.

1. To enable unauthenticated identities select **Enable access to unauthenticated identities** from the **Unauthenticated identities** collapsible section\.

1. If desired, configure an authentication provider in the **Authentication providers** section\. 

1. Choose **Create Pool**\.
**Note**  
At least one identity is required for a valid identity pool\.

1. You will be prompted for access to your AWS resources\.

   Choose **Allow** to create the two default roles associated with your identity pool–one for unauthenticated users and one for authenticated users\. These default roles provide your identity pool access to Amazon Cognito Sync\. You can modify the roles associated with your identity pool in the IAM console\.

## Install the Mobile or JavaScript SDK<a name="install-the-mobile-or-javascript-sdk"></a>

To use Amazon Cognito identity pools, you must install and configure the AWS Mobile or JavaScript SDK\. For more information, see the following topics:
+ [Set Up the AWS Mobile SDK for Android](http://docs.aws.amazon.com/mobile/sdkforandroid/developerguide/setup.html)
+ [Set Up the AWS Mobile SDK for iOS](http://docs.aws.amazon.com/mobile/sdkforios/developerguide/setup-aws-sdk-for-ios.html)
+ [Set Up the AWS SDK for JavaScript](http://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up.html)
+ [Set Up the AWS Mobile SDK for Unity](http://docs.aws.amazon.com/mobile/sdkforunity/developerguide/setup-unity.html)
+ [Set Up the AWS Mobile SDK for \.NET and Xamarin](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/index.html)

## Integrate the Identity Providers<a name="integrate-the-identity-providers"></a>

Amazon Cognito identity pools \(federated identities\) support user authentication through Amazon Cognito user pools, federated identity providers—including Amazon, Facebook, Google, Apple, and SAML identity providers—as well as unauthenticated identities\. This feature also supports [Developer Authenticated Identities \(Identity Pools\)](developer-authenticated-identities.md), which lets you register and authenticate users via your own back\-end authentication process\.

To learn more about using an Amazon Cognito user pool to create your own user directory, see [Amazon Cognito user pools](cognito-user-identity-pools.md) and [Accessing AWS Services Using an Identity Pool After Sign\-in](amazon-cognito-integrating-user-pools-with-identity-pools.md)\.

To learn more about using external identity providers, see [Identity Pools \(Federated Identities\) External Identity Providers](external-identity-providers.md)\.

To learn more about integrating your own back\-end authentication process, see [Developer Authenticated Identities \(Identity Pools\)](developer-authenticated-identities.md)\.

## Get Credentials<a name="get-credentials"></a>

Amazon Cognito identity pools provide temporary AWS credentials for users who are guests \(unauthenticated\) and for users who have authenticated and received a token\. With those AWS credentials your app can securely access a back end in AWS or outside AWS through Amazon API Gateway\. See [Getting Credentials](getting-credentials.md)\.