# Integrating Amazon Cognito With Web and Mobile Apps<a name="cognito-integrate-apps"></a>

When new users discover your app, or when existing users return to it, their first tasks are to sign up or sign in\. By integrating Amazon Cognito with your client code, you connect your app to backend AWS functionality that aids authentication and authorization workflows\. Your app will use the Amazon Cognito API to, for example, create new users in your user pool, retrieve user pool tokens, and obtain temporary credentials from your identity pool\. To integrate Amazon Cognito with your web or mobile app, use the SDKs and libraries that the AWS Amplify framework provides\.

## Amazon Cognito Authentication With the AWS Amplify Framework<a name="cognito-integrate-apps-amplify"></a>

AWS Amplify provides services and libraries for web and mobile developers\. With AWS Amplify, you can build apps that integrate with backend environments that are composed of AWS services\. To provision your backend environment, and to integrate AWS services with your client code, you use the AWS Amplify *framework*\. The framework provides an interactive command line interface \(CLI\) that helps you configure AWS resources for features that are organized into categories, including analytics, storage, and authentication, among many others\. The framework also provides high\-level SDKs and libraries for web and mobile platforms, including iOS, Android, and JavaScript\. Supported JavaScript frameworks include React, React Native, Angular, Ionic, and Vue\. Each of the SDKs and libraries include authentication operations that you can use to implement the authentication workflows that Amazon Cognito drives\.

To use the AWS Amplify framework to add authentication to your app, see the AWS Amplify authorization documentation for your platform:
+ [AWS Amplify authentication for JavaScript](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js)
+ [AWS Amplify authentication for iOS](https://docs.amplify.aws/lib/q/platform/ios)
+ [AWS Amplify authentication for Android](https://docs.amplify.aws/lib/auth/getting-started/q/platform/android)