# Tutorial: Creating a user pool<a name="tutorial-create-user-pool"></a>

With a user pool, your users can sign in to your web or mobile app through Amazon Cognito\.

------
#### [ Original console ]

**To create a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose **Create a user pool**\.

1. Enter a name for your user pool and choose **Review defaults** to save the name\.

1. On the **Review** page, choose **Create pool**\.

------
#### [ New console ]

**To create a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool** to start the user pool creation wizard\.

1. In **Configure sign\-in experience**, choose the federated providers that you will use with this user pool\. For more information, see [Adding User Pool Sign\-in Through a Third Party](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-identity-federation.html)\.

1. In **Configure security requirements**, choose your password policy, multi\-factor authentication \(MFA\) requirements, and user account recovery options\. For more information, see [Security in Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/security.html)\.

1. In **Configure sign\-up experience**, determine how new users will verify their identities when signing up, and which attributes should be required or optional during the user sign\-up flow\. For more information, see [Managing users in user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/managing-users.html)\.

1. In **Configure message delivery**, configure integration with Amazon Simple Email Service and Amazon Simple Notification Service to send email and SMS messages to your users for sign\-up, account confirmation, MFA, and account recovery\. For more information, see [Email Settings for Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-email.html) and [SMS message settings for Amazon Cognito user pools](https://docs.aws.amazon.com/cognito/latest/developerguide/user-pool-sms-settings.html)\.

1. In **Integrate your app**, name your user pool, configure the hosted UI, and create an app client\. For more information, see [Add an App to Enable the Hosted Web UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-app-integration.html)

1. Review your choices in the **Review and create** screen and modify any selections you wish to\. When you are satisfied with your user pool configuration, select **Create user pool** to proceed\.

------

## Related Resources<a name="tutorial-related-resources-1"></a>

For more information on user pools, see [Amazon Cognito user pools](cognito-user-identity-pools.md)\.

See also [User pool authentication flow](amazon-cognito-user-pools-authentication-flow.md) and [Using tokens with user pools](amazon-cognito-user-pools-using-tokens-with-identity-providers.md)\.