# Step 1\. Create a User Pool<a name="cognito-user-pool-as-user-directory"></a>

By using an Amazon Cognito user pool, you can create and maintain a user directory, and add sign\-up and sign\-in to your mobile app or web application\. 

**To create a user pool**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **Manage User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool**\.

1. Provide a name for your user pool, and choose **Review defaults** to save the name\.

1. In the top\-left corner of the page, choose **Attributes**, choose **Email address or phone number** and **Allow email addresses**, and then choose **Next step** to save\.
**Note**  
We recommend that you enable case insensitivity on the `username` attribute before you create your user pool\. For example, when this option is selected, users will be able to sign in using either "username" or "Username"\. Enabling this option also enables both `preferred_username` and `email` alias to be case insensitive, in addition to the `username` attribute\. For more information, see [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) in the *Amazon Cognito user pools API Reference*\. 

1. In the left navigation menu, choose **Review**\.

1. Review the user pool information and make any necessary changes\. When the information is correct, choose **Create pool**\.

## Next Step<a name="cognito-user-pool-as-user-directory-next-step"></a>

[Step 2\. Add an App to Enable the Hosted Web UI](cognito-user-pools-configuring-app-integration.md)