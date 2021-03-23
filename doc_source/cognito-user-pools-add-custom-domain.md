# Using Your Own Domain for the Hosted UI<a name="cognito-user-pools-add-custom-domain"></a>

After setting up an app client, you can configure your user pool with a custom domain for the Amazon Cognito hosted UI\. With a custom domain, you enable your users to sign in to your application by using your own web address\.

**Topics**
+ [Adding a Custom Domain to a User Pool](#cognito-user-pools-add-custom-domain-adding)
+ [Changing the SSL Certificate for Your Custom Domain](#cognito-user-pools-add-custom-domain-changing-certificate)

## Adding a Custom Domain to a User Pool<a name="cognito-user-pools-add-custom-domain-adding"></a>

To add a custom domain to your user pool, you specify the domain name in the Amazon Cognito console, and you provide a certificate you manage with [AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/) \(ACM\)\. After you add your domain, Amazon Cognito provides an alias target, which you add to your DNS configuration\.

### Prerequisites<a name="cognito-user-pools-add-custom-domain-prereq"></a>

Before you begin, you need:
+ A user pool with an app client\. For more information, see [Getting Started with User Pools](getting-started-with-cognito-user-pools.md)\.
+ A web domain that you own\. Its root must have a valid **A record** in DNS\. For more information see [Domain Names](https://tools.ietf.org/html/rfc1035)\.
+ The ability to create a subdomain for your custom domain\. We recommend using **auth** as the subdomain\. For example: *auth\.example\.com*\.
**Note**  
You might need to obtain a new certificate for your custom domain's subdomain if you don't have a [wildcard certificate](https://en.wikipedia.org/wiki/Wildcard_certificate)\.
+ A Secure Sockets Layer \(SSL\) certificate managed by ACM\.
**Note**  
You must change the AWS region to US East \(N\. Virginia\) in the ACM console before you request or import a certificate\. 
+ To set up a custom domain name or to update its certificate, you must have permission to update Amazon CloudFront distributions\. You can do so by attaching the following IAM policy statement to an IAM user, group, or role in your AWS account:

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

  See [Using Identity\-Based Policies \(IAM Policies\) for CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/access-control-managing-permissions.html)\.



### Step 1: Enter Your Custom Domain Name<a name="cognito-user-pools-add-custom-domain-console-step-1"></a>

You can add your domain to your user pool by using the Amazon Cognito console or API\.

**To add your domain to your user pool \(Amazon Cognito console\)**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito/home](https://console.aws.amazon.com/cognito/home)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to add your domain to\.

1. On the navigation menu on the left, choose **Domain name**\.

1. Under **Your own domain**, choose **Use your domain**\.

1. For **Domain name**, enter your custom domain name\. Your domain name can include only lowercase letters, numbers, and hyphens\. Do not use a hyphen for the first or last character\. Use periods to separate subdomain names\.

1. For **AWS managed certificate**, choose the SSL certificate that you want to use for your domain\. You can choose one of the certificates that you manage with ACM\.

   If you don't have a certificate that is available to choose, you can use ACM to provision one\. For more information, see [Getting Started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.

1. Choose **Save changes**\.

1. Note the **Alias target**\. Instead of an IP address or a domain name, the **Alias target** is an alias resource record set that points to an Amazon CloudFront distribution\.

**To add your domain to your user pool \(Amazon Cognito API\)**
+ Use the [CreateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPoolDomain.html) action\.

### Step 2: Add an Alias Target and Subdomain<a name="cognito-user-pools-add-custom-domain-console-step-2"></a>

In this step, you set up an alias through your Domain Name Server \(DNS\) service provider that points back to the alias target from the previous step\. If you are using Amazon Route 53 for DNS address resolution, choose the section **To add an alias target and subdomain using Route 53\.**

#### To add an alias target and subdomain to your current DNS configuration<a name="cognito-user-pools-add-custom-domain-console-step-2a"></a>
+ If you aren't using Route 53 for DNS address resolution, then you need to have your DNS service provider add the alias target from the previous step as an alias for your user pool custom domain\. Your DNS provider will also need to set up the subdomain for your custom domain\.

#### To add an alias target and subdomain using Route 53<a name="cognito-user-pools-add-custom-domain-console-step-2b"></a>

1. Sign in to the [Route 53 console](https://console.aws.amazon.com/route53/)\. You might be prompted for your AWS credentials\.

1. If you don't have a hosted zone in Route 53, set one up\. Otherwise, skip this step\.

   1. Choose **Create Hosted Zone**\.

   1. Choose your custom domain from the **Domain Name** list\.

   1. For **Comment**, type an optional comment about the hosted zone\.

   1. Choose **Create**\.

1. On the **Hosted Zones** page, choose the name of your hosted zone\.

1. Choose **Create Record Set**\.

1. Select **Yes** for the **Alias** option\.

1. Type the alias target name that you noted in a previous step into **Alias Target**\.

1. Choose **Create**\.
**Note**  
Your new records take time to propagate to the Route 53 DNS servers\. Currently, the only way to verify that changes have propagated is to use the Route 53 [GetChange](https://docs.aws.amazon.com/Route53/latest/APIReference/API_GetChange.html) API method\. Changes generally propagate to all Route 53 name servers within 60 seconds\.

1. Add a subdomain in Route 53 by using the alias target\.

   1. On the **Hosted Zones** page, choose the name of your hosted zone\.

   1.  Choose **Create Record Set** and enter the following values:

      1. For **Name**, type your preferred subdomain name\. For example, if the subdomain you’re attempting to create is *auth\.example\.com*, type *auth*\. 

      1. For **Type**, choose **A \- IPv4 address**\.

      1. Select **Yes** for the **Alias** option\.

      1. Type the alias target name that you noted in a previous step in **Alias Target**\.

   1.  Choose **Create**\.
**Note**  
Alternatively, you can create a new hosted zone to hold the records that are associated with your subdomain\. You can also createa delegation set in the parent hosted zone that refers clients to the subdomain hosted zone\. This method offers more flexibility when you're managing the hosted zones \(for example, restricting who can edit the zones\)\. You can only use this method for public hosted zones, because adding NS records to private hosted zones isn't currently supported\. For more information, see [Creating a subdomain for a domain hosted through Amazon Route 53](https://aws.amazon.com/premiumsupport/knowledge-center/create-subdomain-route-53/)\.

### Step 3: Verify Your Sign\-in Page<a name="cognito-user-pools-add-custom-domain-console-step-3"></a>
+ Verify that the sign\-in page is available from your custom domain\.

  Sign in with your custom domain and subdomain by entering this address into your browser\. This is an example URL of a custom domain *example\.com* with the subdomain *auth*:

  ```
  https://auth.example.com/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>
  ```

## Changing the SSL Certificate for Your Custom Domain<a name="cognito-user-pools-add-custom-domain-changing-certificate"></a>

When necessary, you can use Amazon Cognito to change the certificate that you applied to your custom domain\.

Usually, this is unnecessary following routine certificate renewal with ACM\. When you renew your existing certificate in ACM, the ARN for your certificate remains the same, and your custom domain uses the new certificate automatically\.

However, if you replace your existing certificate with a new one, ACM gives the new certificate a new ARN\. To apply the new certificate to your custom domain, you must provide this ARN to Amazon Cognito\.

After you provide your new certificate, Amazon Cognito requires up to 1 hour to distribute it to your custom domain\.

**Before you begin**  
Before you can change your certificate in Amazon Cognito, you must add your certificate to ACM\. For more information, see [Getting Started](https://docs.aws.amazon.com/acm/latest/userguide/gs.html) in the *AWS Certificate Manager User Guide*\.  
When you add your certificate to ACM, you must choose US East \(N\. Virginia\) as the AWS Region\.

You can change your certificate by using the Amazon Cognito console or API\.

**To renew a certificate \(Amazon Cognito console\)**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console.aws.amazon.com/cognito/home](https://console.aws.amazon.com/cognito/home)\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to add your domain to\.

1. On the navigation menu on the left, choose **Domain name**\.

1. Under **Your own domain**, for **AWS managed certificate**, choose your new certificate\.

1. Choose **Save changes**\.

**To renew a certificate \(Amazon Cognito API\)**
+ Use the [UpdateUserPoolDomain](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPoolDomain.html) action\.