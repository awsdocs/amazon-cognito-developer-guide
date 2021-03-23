# Using the Amazon Cognito Domain for the Hosted UI<a name="cognito-user-pools-assign-domain-prefix"></a>

After setting up an app client, you can configure the address of your sign\-up and sign\-in webpages\. You can use the hosted Amazon Cognito domain with your own domain prefix\.

To add an app client and an Amazon Cognito hosted domain with the AWS Management Console, see [ Adding an App to Enable the Hosted Web UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-configuring-app-integration.html)\.

**Topics**
+ [Prerequisites](#cognito-user-pools-assign-domain-prefix-prereq)
+ [Step 1: Configure a Hosted User Pool Domain](#cognito-user-pools-assign-domain-prefix-step-1)
+ [Step 2: Verify Your Sign\-in Page](#cognito-user-pools-assign-domain-prefix-verify)

## Prerequisites<a name="cognito-user-pools-assign-domain-prefix-prereq"></a>

Before you begin, you need:
+ A user pool with an app client\. For more information, see [Getting Started with User Pools](getting-started-with-cognito-user-pools.md)\.

## Step 1: Configure a Hosted User Pool Domain<a name="cognito-user-pools-assign-domain-prefix-step-1"></a>

### To configure a hosted user pool domain \(AWS Management Console\)<a name="cognito-user-pools-assign-domain-prefix-console"></a>

You can use the AWS Management Console to configure a user pool domain\.

**To configure a Amazon Cognito hosted domain**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, choose **Manage your User Pools**, and choose the user pool you want to edit\.

1. Choose the **Domain name** tab\.

1. Type the domain prefix you want to use in the **Prefix domain name** box\.

1. Choose **Check availability** to confirm the domain prefix is available\.

1. Choose **Save changes**\.

### To configure a hosted user pool domain \(AWS CLI and AWS API\)<a name="cognito-user-pools-assign-domain-prefix-cli-api"></a>

Use the following commands to create a domain prefix and assign it to your user pool\.

**To configure a user pool domain**
+ AWS CLI: `aws cognito-idp create-user-pool-domain`

  Example: `aws cognito-idp create-user-pool-domain --user-pool-id <user_pool_id> --domain <domain_name>`
+ AWS API: [CreateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolDomain.html)

**To get information about a domain**
+ AWS CLI: `aws cognito-idp describe-user-pool-domain`

  Example: `aws cognito-idp describe-user-pool-domain --domain <domain_name>`
+ AWS API: [DescribeUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DescribeUserPoolDomain.html)

**To delete a domain**
+ AWS CLI: `aws cognito-idp delete-user-pool-domain`

  Example: `aws cognito-idp delete-user-pool-domain --domain <domain_name>`
+ AWS API: [DeleteUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_DeleteUserPoolDomain.html)

## Step 2: Verify Your Sign\-in Page<a name="cognito-user-pools-assign-domain-prefix-verify"></a>
+ Verify that the sign\-in page is available from your Amazon Cognito hosted domain\.

  ```
  https://your_domain/login?response_type=code&client_id=your_app_client_id&redirect_uri=your_callback_url
  ```

Your domain is shown on the **Domain name** page of the Amazon Cognito console\. Your app client ID and callback URL are shown on the **App client settings** page\.