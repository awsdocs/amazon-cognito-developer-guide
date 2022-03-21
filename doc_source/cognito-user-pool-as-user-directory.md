# Step 1\. Create a user pool<a name="cognito-user-pool-as-user-directory"></a>

Using an Amazon Cognito user pool, you can create and maintain a user directory, and add sign\-up and sign\-in to your mobile app or web application\. 

------
#### [ Original console ]

**To create a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool**\.

1. Enter a name for your user pool, and choose **Review defaults** to save the name\.

1. In the top\-left corner of the page, choose **Attributes**, choose **Email address or phone number** and **Allow email addresses**, and then choose **Next step** to save\.
**Note**  
We recommend that you enable case insensitivity on the `username` attribute before you create your user pool\. For example, when this option is selected, users will be able to sign in using either "**username**" or "**Username**"\. Enabling this option also enables both `preferred_username` and `email` alias to be case insensitive, in addition to the `username` attribute\. For more information, see [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) in the *Amazon Cognito user pools API Reference*\. 

1. In the left navigation menu, choose **Review**\.

1. Review the user pool information and make any necessary changes\. When the information is correct, choose **Create pool**\.

------
#### [ New console ]

**To create a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool** to start the user pool creation wizard\.

1. In **Configure sign\-in experience**, choose the federated providers that you want to use with this user pool\. For more information, see [Adding User Pool Sign\-in Through a Third Party](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-identity-federation.html)\.
**Note**  
The **Make user name case sensitive** option is turned off by default\. We recommend that you do not activate this option\. When the user name is not case sensitive, users can sign in with either **username** or **Username**\. The **Make user name case sensitive** option also governs case sensitivity of the `preferred_username` and `email` aliases\. When user name is case sensitive, you must take additional security precautions\. For more information, see [User pool case sensitivity](user-pool-case-sensitivity.md)\. 

1. In **Configure security requirements**, choose your password policy, multi\-factor authentication \(MFA\) requirements, and user account recovery options\. For more information, see [Security in Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/security.html)\.

1. In **Configure sign\-up experience**, determine how new users will verify their identities when signing up, and which attributes should be required or optional during the user sign\-up flow\. For more information, see [Managing users in user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/managing-users.html)\.

1. In **Configure message delivery**, configure integration with Amazon Simple Email Service \(Amazon SES\) and Amazon Simple Notification Service \(Amazon SNS\) to send email and SMS messages to your users for sign\-up, account confirmation, MFA, and account recovery\. For more information, see [Email Settings for Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-email.html) and [SMS message settings for Amazon Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sms-settings.html)\.

1. In **Integrate your app**, name your user pool, configure the hosted UI, and create an app client\. For more information, see [Add an App to Enable the Hosted Web UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-app-integration.html)

1. Review your choices in the **Review and create** screen and modify any selections you wish to\. When you are satisfied with your user pool configuration, select **Create user pool** to proceed\.

------

## Next Step<a name="cognito-user-pool-as-user-directory-next-step"></a>

[Step 2\. Add an app to enable the hosted web UI](cognito-user-pools-configuring-app-integration.md)