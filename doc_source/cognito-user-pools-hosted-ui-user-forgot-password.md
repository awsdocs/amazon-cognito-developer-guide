# How to reset a password with the Amazon Cognito hosted UI<a name="cognito-user-pools-hosted-ui-user-forgot-password"></a>

This guide shows you how to reset your password in apps that use Amazon Cognito\.

**Note**  
When you sign in to an app that uses the Amazon Cognito hosted user interface \(UI\), you might see a page that the app owner has customized beyond the basic configuration shown in this guide\.

1. Depending on the options that the app owner has chosen, you might have a choice of providers to sign in with, or you might only see a prompt for a user name and password\. When you sign in with a user name and password from this page, Amazon Cognito is your sign\-in provider\. Otherwise, your sign\-in provider is represented by the button you choose\.

   If you normally choose a provider from the sign\-in page, and your password isn't working, follow the procedure to reset your password with the provider\. If Amazon Cognito is your sign\-in provider, choose **Forgot your password?**

------
#### [ With multiple sign\-in providers ]

![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with several sign-in providers\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ With only Amazon Cognito as a sign\-in provider ]

![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page with only an Amazon Cognito sign-in provider\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------

1. On the **Forgot your password?** page, Amazon Cognito prompts you for the information that you use to sign in\. This might be your user name, email address, or phone number\.  
![\[hosted UI forgot-password page with a prompt for user name\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI forgot-password page with a prompt for user name\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI forgot-password page with a prompt for user name\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. Amazon Cognito will send a code to you as an email message or as an SMS text message\.

   Enter the code that you received, and enter your new password twice in the fields provided\.  
![\[hosted UI forgot-password page with a prompt for reset code and new password\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI forgot-password page with a prompt for reset code and new password\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI forgot-password page with a prompt for reset code and new password\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

1. After you change your password, return to the sign\-in page and sign in with your new password\.