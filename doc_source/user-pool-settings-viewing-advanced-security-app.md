# Enabling user pool advanced security from your app<a name="user-pool-settings-viewing-advanced-security-app"></a>

After you configure the advanced security features for your user pool, you must activate them in your web or mobile app\.

## To use advanced security with JavaScript<a name="user-pool-settings-viewing-advanced-security-app-javascript"></a>

1. You might need to update your Amazon Cognito SDK to the latest version\. For more information about Amazon Cognito SDKs, see [Install a user pool SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

1. To use the Auth SDK to activate the hosted UI, see the [CognitoAuth JavaScript sample app](https://github.com/aws/amazon-cognito-auth-js/tree/master/sample)\.

1. Set `AdvancedSecurityDataCollectionFlag` to `true`\. Also, set `UserPoolId` to your user pool ID\.

1. In your application replace <region> with your AWS Region such as us\-east\-1, and add this source reference to your JavaScript file:

   ```
   <script src="https://amazon-cognito-assets.<region>.amazoncognito.com/amazon-cognito-advanced-security-data.min.js"></script>
   ```

   For more information, see [Sample for Amazon Cognito Auth SDK for JavaScript](https://github.com/aws/amazon-cognito-auth-js/blob/master/sample/SAMPLEREADME.md)\.

## To use advanced security with Android<a name="user-pool-settings-viewing-advanced-security-app-android"></a>

1. You might need to update your Amazon Cognito SDK to the latest version\. For more information about Amazon Cognito SDKs, see [Install a user pool SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

1. To use the Auth SDK to activate the hosted UI, see the [CognitoAuth Android sample app](https://github.com/awslabs/aws-sdk-android-samples/tree/master/AmazonCognitoAuthDemo)\.

1. When you import `aws-android-sdk-cognitoauth` through Maven in Gradle, use `{ transitive = true; }` \.

   Include the following dependency in your build\.gradle file:

   ```
   compile "com.amazonaws:aws-android-sdk-cognitoidentityprovider-asf:1.0.0"
   ```

   For more information, see [AWS SDK for Android \- Amazon Cognito Identity Provider ASF](https://javalibs.com/artifact/com.amazonaws/aws-android-sdk-cognitoidentityprovider-asf)\.

## To use advanced security with iOS<a name="user-pool-settings-viewing-advanced-security-app-ios"></a>

1. You might need to update your Amazon Cognito SDK to the latest version\. For more information about Amazon Cognito SDKs, see [Install a user pool SDK](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

1. To use the Auth SDK to activate the hosted UI, see the [CognitoAuth iOS sample app](https://github.com/awslabs/aws-sdk-ios-samples/tree/master/CognitoAuth-Sample)\.

1. To configure the Auth SDK by using Info\.plist, add the PoolIdForEnablingASF key to your Amazon Cognito user pool configuration\. Set the key to your user pool ID\.

   To configure the Auth SDK using AWSCognitoAuthConfiguration, use [ this initializer](https://docs.aws.amazon.com/AWSiOSSDK/latest/Classes/AWSCognitoAuthConfiguration.html#/api/name/initWithAppClientId:appClientSecret:scopes:signInRedirectUri:signOutRedirectUri:webDomain:identityProvider:idpIdentifier:userPoolIdForEnablingASF:)\. Specify your user pool ID as userPoolIdForEnablingASF\.

   For more information, see [AWSCognitoIdentityProviderASF](https://github.com/aws/aws-sdk-ios/tree/master/AWSCognitoIdentityProviderASF)\.