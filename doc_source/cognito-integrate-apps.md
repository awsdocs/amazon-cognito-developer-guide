# Integrating Amazon Cognito with web and mobile apps<a name="cognito-integrate-apps"></a>

When new users discover your app, or when existing users return to it, their first task is to sign up or sign in\. When you integrate Amazon Cognito with your client code, you connect your app to AWS resources that aid authentication and authorization workflows\. For example, your app uses the Amazon Cognito API to create new users in your user pool, retrieve user pool tokens, and obtain temporary credentials from your identity pool\. To integrate Amazon Cognito with your web or mobile app, use AWS SDKs and libraries\. 

Although Amazon Cognito offers visual tools such as AWS Management Console integration and the hosted UI, AWS has designed the service to work with your app code\. You can only configure certain components of Amazon Cognito with the API or the AWS Command Line Interface\. For example, you can only register a user for time\-based one\-time password \(TOTP\) multi\-factor authentication \(MFA\) with a process that starts with [AssociateSoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AssociateSoftwareToken.html)\. Before you use Amazon Cognito authentication and authorization, choose an app platform and prepare your code to integrate with the service\.

## Amazon Cognito authentication with the AWS Amplify framework<a name="cognito-integrate-apps-amplify"></a>

AWS Amplify provides services and libraries for web and mobile developers\. With Amplify, you can build apps that integrate with backend environments that AWS services compose\. To provision your backend environment, and to integrate AWS services with your client code, use the Amplify *framework*\. The framework provides an interactive command line interface \(CLI\) that helps you configure AWS resources for features that are organized into categories\. These categories include analytics, storage, and authentication, among many others\. The framework also provides high\-level SDKs and libraries for web and mobile platforms, including iOS, Android, and JavaScript\. The JavaScript frameworks include React, React Native, Angular, Ionic, and Vue\. Each of the SDKs and libraries includes authentication operations that you can use to implement the authentication workflows that Amazon Cognito drives\.

To use the Amplify framework to add authentication to your app, see the Amplify authorization documentation for your platform:
+ [Amplify authentication for JavaScript](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js)
+ [Amplify authentication for iOS](https://docs.amplify.aws/lib/q/platform/ios)
+ [Amplify authentication for Android](https://docs.amplify.aws/lib/auth/getting-started/q/platform/android)

## Authentication with amazon\-cognito\-identity\-js<a name="amazon-cognito-user-pools-authentication-using-amazon-cognito-identity-js"></a>

The Amazon Cognito Identity SDK for JavaScript makes it possible for apps to sign up and authenticate users in Amazon Cognito user pools\. Apps can also use this SDK to view, delete, and update user attributes in Amazon Cognito user pools\. The *amazon\-cognito\-identity\-js* package provides sample code that makes it possible for authenticated users to change their passwords\. The package also provides sample code that can initiate and complete recovery of forgotten passwords for unauthenticated users\.

[Amazon Cognito Identity SDK for JavaScript](https://github.com/aws-amplify/amplify-js/tree/main/packages/amazon-cognito-identity-js)

## Authentication with AWS SDKs<a name="amazon-cognito-authentication-with-sdks"></a>

If you must use a secure backend to build your own identity microservice that interacts with Amazon Cognito, you can use the Amazon Cognito user pools and Amazon Cognito federated identities API\.

For details on each API operation, see the [Amazon Cognito user pools API Reference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_Operations.html) and the [Amazon Cognito API Reference](https://docs.aws.amazon.com/cognitoidentity/latest/APIReference/Welcome.html)\. These documents contain [see also](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html#API_InitiateAuth_SeeAlso) sections with resources for using a variety of SDKs in supported platforms\.