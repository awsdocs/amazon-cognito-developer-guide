# How to sign in with the Amazon Cognito hosted UI<a name="cognito-user-pools-hosted-ui-user-sign-in"></a>

This guide shows you how to sign in to apps that use Amazon Cognito\.

**Note**  
When you sign in to an app that uses the Amazon Cognito hosted user interface \(UI\), you might see a page that the app owner has customized beyond the basic configuration shown in this guide\.

1. Depending on the options that the app owner has chosen, you might have a choice of providers to sign in with, or you might only see a prompt for a user name and password\. When you sign in with a user name and password from this page, Amazon Cognito is your sign\-in provider\. Otherwise, your sign\-in provider is represented by the button you choose\.

   You might choose a provider here, or enter a user name and password, and get access to your app immediately\. If Amazon Cognito is your sign\-in provider, the app owner might also require multi\-factor authentication\.

------
#### [ With multiple sign\-in providers ]

![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ With only Amazon Cognito as a sign\-in provider ]

![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------

1. You might have set up MFA when you signed up in the app\. Enter your MFA code that you either received in an SMS message, or is displayed in your authenticator app\.

------
#### [ With an authenticator app ]

![\[hosted UI sign-in page a prompt for a password from a mobile authenticator app\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page a prompt for a password from a mobile authenticator app\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page a prompt for a password from a mobile authenticator app\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ With an SMS code ]

![\[hosted UI sign-in page with a prompt for a code from an SMS message\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with a prompt for a code from an SMS message\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with a prompt for a code from an SMS message\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------

1. After you sign in and complete MFA, Amazon Cognito grants access to your app\.