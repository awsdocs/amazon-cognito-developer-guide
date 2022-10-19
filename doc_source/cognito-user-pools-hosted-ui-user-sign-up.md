# How to sign up for a new account in the Amazon Cognito hosted UI<a name="cognito-user-pools-hosted-ui-user-sign-up"></a>

This guide shows you how to sign up for a user account in apps that use Amazon Cognito\.

**Note**  
When you sign in to an app that uses the Amazon Cognito hosted user interface \(UI\), you might see a page that the app owner has customized beyond the basic configuration shown in this guide\.

1. Choose **Sign up** from the sign\-in page if you intend to sign in through Amazon Cognito with a user name and password, instead of one of the third\-party sign\-in providers that the app owner has listed\. 

   If your sign\-in provider is anything other than Amazon Cognito, your sign\-up is complete after you choose the button for your third\-party provider\. Depending on the options that the app owner has chosen, you might have a choice of providers to sign in with, or you might only see a prompt for a user name and password\.

------
#### [ With multiple sign\-in providers ]

![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ With only Amazon Cognito as a sign\-in provider ]

![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------

1. On the **Sign up with a new account** page, the app owner asks for the information that they require to sign up\. They might ask for a user name, an email address, or a phone number\. Enter the required information and choose a password\.  
![\[hosted UI sign-up requesting user name, email address, and password.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-up requesting user name, email address, and password.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-up requesting user name, email address, and password.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. On the **Confirm your account** page, the app owner might require you to confirm your account to verify that you can receive messages at the email address or phone number you provided\.

   You'll receive a code in your email or in an SMS text message\. Enter the code in the form to confirm that you entered the correct contact information\.  
![\[hosted UI sign-in page a prompt for a password from a mobile authenticator app\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page a prompt for a password from a mobile authenticator app\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page a prompt for a password from a mobile authenticator app\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. The app owner might require that you set up multi\-factor authentication \(MFA\)\. You might see a prompt to choose your MFA method, or your app might skip to the next step\.

   On the **Choose a multi\-factor authentication \(MFA\) method **page, choose an MFA method\. If you choose **SMS**, you will receive MFA passcodes in SMS text messages\. If you choose **Authenticator app**, you must install an app on your device to generate time\-based MFA passcodes\.  
![\[hosted UI sign-up presenting a choice of SMS or authenticator app multi-factor authentication.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-up presenting a choice of SMS or authenticator app multi-factor authentication.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-up presenting a choice of SMS or authenticator app multi-factor authentication.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. Amazon Cognito asks you for a code from your authenticator app or SMS text message\. Enter the code that you received\.

------
#### [ Authenticator app ]

   1. Open the authenticator app that you downloaded\.

   1. Scan the QR code on the page with your camera\. You might need to authorize the app to use your camera\.

      If you can't scan the QR code, choose **Show secret key** to display a code that you can enter manually into your authenticator app\.

   1. Your authenticator app begins to display codes that changes every several seconds\. Enter a current code from the app\.

   1. \(Optional\) On the **Set up authenticator app MFA** page, choose a name for your device\. When you sign in, Amazon Cognito will ask you for a code from the device with the name that you give it here\.

![\[hosted UI sign-in page displaying authenticator app multi-factor authentication setup\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page displaying authenticator app multi-factor authentication setup\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page displaying authenticator app multi-factor authentication setup\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ SMS text message ]

   1. If the app owner hasn't already collected your phone number, Amazon Cognito requests your phone number\.

      On the **Set up SMS MFA** page, enter a phone number that includes a `+` sign and a country code, for example `+12125551234`\.  
![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

   1. You'll receive an SMS message with a code\. On the **Set up SMS MFA** page, enter the code\. If you didn't get a code and you want to try again, choose **Send a new code**\. Select **Back** to enter a new phone number\.  
![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------

1. The first time that you sign up and confirm your details, Amazon Cognito grants access to your app after you finish this process\.