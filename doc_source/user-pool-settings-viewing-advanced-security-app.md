# Activating user pool advanced security from your app<a name="user-pool-settings-viewing-advanced-security-app"></a>

After you configure the advanced security features for your user pool, you must activate them in your web or mobile app\.

## Using advanced security with JavaScript<a name="user-pool-settings-viewing-advanced-security-app-javascript"></a>

1. You might need to update your Amazon Cognito SDK to the latest version\. For more information about Amazon Cognito SDKs, see [Integrating Amazon Cognito with web and mobile apps](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

1. To use the Auth SDK to activate the hosted UI, see [Amazon Cognito Identity SDK for JavaScript](https://github.com/aws-amplify/amplify-js/tree/main/packages/amazon-cognito-identity-js)\.

1. Set `AdvancedSecurityDataCollectionFlag` to `true`\. Also, set `UserPoolId` to your user pool ID\.

1. Add this source reference to your app's JavaScript file\. Replace `<region>` with an AWS Region like `us-east-1`\.

   ```
   <script src="https://amazon-cognito-assets.<region>.amazoncognito.com/amazon-cognito-advanced-security-data.min.js"></script>
   ```

   For more information, see [Amazon Cognito Identity SDK for JavaScript](https://github.com/aws-amplify/amplify-js/tree/main/packages/amazon-cognito-identity-js)\.

## Using advanced security with Android<a name="user-pool-settings-viewing-advanced-security-app-android"></a>

1. You might need to update your Amazon Cognito SDK to the latest version\. For more information about Amazon Cognito SDKs, see [Integrating Amazon Cognito with web and mobile apps](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

1. To use the AWS Amplify SDK to activate the hosted UI, see the **Authentication** topic in the [Amplify Docs](https://docs.amplify.aws/sdk/auth/hosted-ui/q/platform/android/)\.

1. When you import `aws-android-sdk-cognitoauth` through Maven in Gradle, use `{ transitive = true; }` \.

   Include the following dependency in your build\.gradle file:

   ```
   compile "com.amazonaws:aws-android-sdk-cognitoidentityprovider-asf:1.0.0"
   ```

   For more information, see [aws\-android\-sdk\-cognitoidentityprovider\-asf](https://github.com/aws-amplify/aws-sdk-android/tree/main/aws-android-sdk-cognitoidentityprovider-asf)\.

## Using advanced security with iOS<a name="user-pool-settings-viewing-advanced-security-app-ios"></a>

1. You might need to update your Amazon Cognito SDK to the latest version\. For more information about Amazon Cognito SDKs, see [Integrating Amazon Cognito with web and mobile apps](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sdk-links.html)\.

1. To use the AWS Amplify SDK to activate the hosted UI, see the **Authentication** topic in the [Amplify Docs](https://docs.amplify.aws/sdk/auth/hosted-ui/q/platform/ios/)\.

1. To configure the Auth SDK with Info\.plist, add the PoolIdForEnablingASF key to your Amazon Cognito user pool configuration\. Set the key to your user pool ID\.

   For more information, see [https://github.com/aws/aws-sdk-ios/tree/master/AWSCognitoIdentityProviderASF](https://github.com/aws/aws-sdk-ios/tree/master/AWSCognitoIdentityProviderASF)\.