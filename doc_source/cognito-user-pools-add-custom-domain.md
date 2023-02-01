# Using your own domain for the hosted UI<a name="cognito-user-pools-add-custom-domain"></a>

After setting up an app client, you can configure your user pool with a custom domain for the Amazon Cognito hosted UI and [auth API](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html) endpoints\. With a custom domain, you enable your users to sign in to your application by using your own web address\.

**Topics**
+ [Adding a custom domain to a user pool](#cognito-user-pools-add-custom-domain-adding)
+ [Changing the SSL certificate for your custom domain](#cognito-user-pools-add-custom-domain-changing-certificate)

## Adding a custom domain to a user pool<a name="cognito-user-pools-add-custom-domain-adding"></a>

To add a custom domain to your user pool, you specify the domain name in the Amazon Cognito console, and you provide a certificate you manage with [AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/) \(ACM\)\. After you add your domain, Amazon Cognito provides an alias target, which you add to your DNS configuration\.

### Prerequisites<a name="cognito-user-pools-add-custom-domain-prereq"></a>

Before you begin, you need:
+ A user pool with an app client\. For more information, see [Getting started with user pools](getting-started-with-cognito-user-pools.md)\.
+ A web domain that you own\. Its *parent domain* must have a valid **A record** in DNS\. The parent may be the root of the domain, or a child domain that is one step up in the domain hierarchy\. For example, if your custom domain is *auth\.xyz\.example\.com*, Amazon Cognito must be able to resolve *xyz\.example\.com* to an IP address\. To prevent accidental impact on customer infrastructure, Amazon Cognito doesn't support the use of top\-level domains \(TLDs\) for custom domains\. For more information see [Domain Names](https://tools.ietf.org/html/rfc1035)\.
+ The ability to create a subdomain for your custom domain\. We recommend using **auth** as the subdomain\. For example: *auth\.example\.com*\.
**Note**  
You might need to obtain a new certificate for your custom domain's subdomain if you don't have a [wildcard certificate](https://en.wikipedia.org/wiki/Wildcard_certificate)\.
+ A Secure Sockets Layer \(SSL\) certificate managed by ACM\.
**Note**  
You must change the AWS region to US East \(N\. Virginia\) in the ACM console before you request or import a certificate\. 
+ To set up a custom domain name or to update its certificate, you must have permission to update Amazon CloudFront distributions\. You can do so by attaching the following IAM policy statement to a user in your AWS account:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
           {
              "Sid": "AllowCloudFrontUpdateDistribution",
              "Effect": "Allow",
              "Action": [
                  "cloudfront:updateDistribution"
              ],
              "Resource": [
                  "*"
              ]
          }
      ]
  }
  ```

  For more information about authorizing actions in CloudFront, see [Using Identity\-Based Policies \(IAM Policies\) for CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/access-control-managing-permissions.html)\.

  Amazon Cognito initially uses your IAM permissions to configure the CloudFront distribution, but the distribution is managed by AWS\. You can't change the configuration of the CloudFront distribution that Amazon Cognito associated with your user pool\. For example, you can't update the supported TLS versions in the security policy\.

### Step 1: Enter your custom domain name<a name="cognito-user-pools-add-custom-domain-console-step-1"></a>

You can add your domain to your user pool by using the Amazon Cognito console or API\.

------
#### [ Original console ]

**To add your domain to your user pool from the Amazon Cognito console:**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool to which you want to add your domain\.

1. In the navigation menu on the left, choose **Domain name**\.

1. Under **Your own domain**, choose **Use your domain**\.

1. For **Domain name**, enter your custom domain name\. Your domain name can include only lowercase letters, numbers, and hyphens\. Do not use a hyphen for the first or last character\. Use periods to separate subdomain names\.

1. For **AWS managed certificate**, choose the SSL certificate that you want to use for your domain\. You can choose one of the certificates that you manage with ACM\.

   If you don't have a certificate that is available to choose, you can use ACM to provision one\. For more information, see [Getting Started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.

1. Choose **Save changes**\.

**Note**  
Make sure to note the **Alias target** for your domain\. Instead of an IP address or a domain name, the **Alias target** is an alias resource record set that points to an Amazon CloudFront distribution\. You will use the **Alias target** when configuring DNS settings with Amazon Route 53 or a third\-party DNS provider\.

------
#### [ New console ]

**To add your domain to your user pool from the Amazon Cognito console:**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User pools**\.

1. Choose the user pool to which you want to add your domain\.

1. Choose the **App integration** tab\.

1. Next to **Domain**, choose **Actions**, then choose **Create custom domain**\. 
**Note**  
If you have already configured a user pool domain, choose **Delete Cognito domain** or **Delete custom domain** to delete the existing domain before creating your new custom domain\.

1. For **Custom domain**, enter the URL of the domain you want to use with Amazon Cognito\. Your domain name can include only lowercase letters, numbers, and hyphens\. Do not use a hyphen for the first or last character\. Use periods to separate subdomain names\.

1. For **ACM certificate**, choose the SSL certificate that you want to use for your domain\. Only ACM certificates in US East \(N\. Virginia\) are eligible to use with an Amazon Cognito custom domain, regardless of the AWS Region of your user pool\.

   If you don't have an available certificate, you can use ACM to provision one in US East \(N\. Virginia\)\. For more information, see [Getting Started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.

1. Choose **Create**\.

1. Amazon Cognito returns you to the **App integration** tab\. A message titled **Create an alias record in your domain's DNS** is displayed\. Note down the **Domain** and **Alias target** displayed in the console\. They will be used in the next step to direct traffic to your custom domain\.

------
#### [ API ]

**To add your domain to your user pool with the Amazon Cognito API:**
+ Use the [CreateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolDomain.html) action\.

------

### Step 2: Add an alias target and subdomain<a name="cognito-user-pools-add-custom-domain-console-step-2"></a>

In this step, you set up an alias through your Domain Name Server \(DNS\) service provider that points back to the alias target from the previous step\. If you are using Amazon Route 53 for DNS address resolution, choose the section **To add an alias target and subdomain using Route 53\.**

#### To add an alias target and subdomain to your current DNS configuration<a name="cognito-user-pools-add-custom-domain-console-step-2a"></a>
+ If you aren't using Route 53 for DNS address resolution, then you must use your DNS service provider's configuration tools to add the alias target from the previous step to your domain's DNS record\. Your DNS provider will also need to set up the subdomain for your custom domain\.

#### To add an alias target and subdomain using Route 53<a name="cognito-user-pools-add-custom-domain-console-step-2b"></a>

1. Sign in to the [Route 53 console](https://console.aws.amazon.com/route53/)\. If prompted, enter your AWS credentials\.

1. If you don't have a hosted zone in Route 53, you must create one using the following steps\. Otherwise, continue to step 3\.

   1. Choose **Create Hosted Zone**\.

   1. Enter the parent domain, for example *auth\.example\.com*, of your custom domain, for example *myapp\.auth\.example\.com*, from the **Domain Name** list\.

   1. \{Optional\} For **Comment**, enter a comment about the hosted zone\.

   1. Choose a hosted zone **Type** of **Public hosted zone** to allow public clients to resolve your custom domain\. Choosing **Private hosted zone** is not supported\.

   1. Apply **Tags** as desired\.

   1. Choose **Create hosted zone**\.
**Note**  
You can also create a new hosted zone for your custom domain, and you can create a delegation set in the parent hosted zone that directs queries to the subdomain hosted zone\. This method offers more flexibility and security with your hosted zones\.For more information, see [Creating a subdomain for a domain hosted through Amazon Route 53](https://aws.amazon.com/premiumsupport/knowledge-center/create-subdomain-route-53/)\.

1. On the **Hosted Zones** page, choose the name of your hosted zone\.

1. Choose **Create record**\. Add a DNS record for the parent domain and choose **Create records**\. The following is an example record for the domain *auth\.example\.com*\.

   `auth.example.com. 60 IN A 198.51.100.1`
**Note**  
Amazon Cognito verifies that there is a DNS record for the parent domain of your custom domain to protect against accidental hijacking of production domains\. If you do not have a DNS record for the parent domain, Amazon Cognito will return an error when you attempt to set the custom domain\.

1. Choose **Create record** again\.

1. Enter a **Record name** that matches your custom domain, for example *myapp* to create a record for *myapp\.auth\.example\.com*\.

1. Enter the alias target name that you previously noted into **Alias Target**\.

1. Enable the **Alias** option\.

1. Choose to **Route traffic to** an **Alias to Cloudfront distribution**\. Enter the **Alias target** provided by Amazon Cognito when you created your custom domain\.

1. Choose **Create Records**\.
**Note**  
Your new records can take around 60 seconds to propagate to all Route 53 DNS servers\. You can use the Route 53 [GetChange](https://docs.aws.amazon.com/Route53/latest/APIReference/API_GetChange.html) API method to verify that your changes have propagated\. 

### Step 3: Verify your sign\-in page<a name="cognito-user-pools-add-custom-domain-console-step-3"></a>
+ Verify that the sign\-in page is available from your custom domain\.

  Sign in with your custom domain and subdomain by entering this address into your browser\. This is an example URL of a custom domain *example\.com* with the subdomain *auth*:

  ```
  https://myapp.auth.example.com/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>
  ```

## Changing the SSL certificate for your custom domain<a name="cognito-user-pools-add-custom-domain-changing-certificate"></a>

When necessary, you can use Amazon Cognito to change the certificate that you applied to your custom domain\.

Usually, this is unnecessary following routine certificate renewal with ACM\. When you renew your existing certificate in ACM, the ARN for your certificate remains the same, and your custom domain uses the new certificate automatically\.

However, if you replace your existing certificate with a new one, ACM gives the new certificate a new ARN\. To apply the new certificate to your custom domain, you must provide this ARN to Amazon Cognito\.

After you provide your new certificate, Amazon Cognito requires up to 1 hour to distribute it to your custom domain\.

**Before you begin**  
Before you can change your certificate in Amazon Cognito, you must add your certificate to ACM\. For more information, see [Getting Started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.  
When you add your certificate to ACM, you must choose US East \(N\. Virginia\) as the AWS Region\.

You can change your certificate by using the Amazon Cognito console or API\.

------
#### [ Original console ]

**To renew a certificate from the Amazon Cognito console:**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito/home](https://console.aws.amazon.com/cognito/home)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool to which you want to add your domain\.

1. On the navigation menu on the left, choose **Domain name**\.

1. Under **Your own domain**, for **AWS managed certificate**, choose your new certificate\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To renew a certificate from the Amazon Cognito console:**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito/home](https://console.aws.amazon.com/cognito/home)\.

1. Choose **User Pools**\.

1. Choose the user pool for which you want to update the certificate\.

1. Choose the **App integration** tab\.

1. Choose **Actions**, **Edit ACM certificate**\.

1. Select the new certificate you want to associate with your custom domain\.

1. Choose **Save changes**\.

------
#### [ API ]

**To renew a certificate \(Amazon Cognito API\)**
+ Use the [UpdateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolDomain.html) action\.

------