# Document History for Amazon Cognito<a name="cognito-document-history"></a>

The following table describes the documentation for this release of Amazon Cognito\.
+ **Original API versions:**

  Amazon Cognito User Pools: 2021\-1\-15

  Amazon Cognito Federated Identities: 2014\-06\-30

  Amazon Cognito Sync: 2014\-06\-30
+ **Latest documentation update:**March 4, 2021


****  

| Change | Description | Date | 
| --- | --- | --- | 
| Publication of guide markdown to GitHub | As with all of AWS documentation, this guide now has markdown available to review and comment on in at [https://github.com/awsdocs/amazon-cognito-developer-guide](https://github.com/awsdocs/amazon-cognito-developer-guide)\. | March 23, 2021 | 
| Multi\-tenant nest practices |  Best practices for multi\-tenant applications were added to the documentation\.  | March 4, 2021 | 
| Attributes for access control | Amazon Cognito Identity Pools provide attributes for access control \(AFAC\) as a way for customers to grant users access to AWS resources\. Authorization can be done based on users' attributes from the identity provider which they used to federate with Amazon Cognito\.  | January 15, 2021 | 
| Custom SMS Sender Lambda Trigger and Custom Email Sender Lambda Trigger | The Custom SMS Sender Lambda Trigger and Custom Email Sender Lambda Trigger allow you to enable a third\-party provider to send email and SMS notifications to your users from within your Lambda function code\.  | November 2020 | 
| Amazon Cognito token updates | Updated expiration information was added to Access, ID, and Refresh tokens\.  | October 29, 2020 | 
| Amazon Cognito Service Quotas | Service Quotas are available for Amazon Cognito category quotas\. You can use the Service Quotas console to view quota usage, request a quota increase, and create CloudWatch alarms to monitor your quota usage\. As part of this change the Available CloudWatch Metrics for Amazon Cognito User Pools section was updated to reflect the new information\. The new section name is: Tracking quotas and usage in CloudWatch and Service Quotas  | October 29, 2020 | 
| Amazon Cognito quota categorization | Quota categories are available to help you monitor quota usage and request an increase\. The quotas are grouped into categories based on common use cases\.  | August 17, 2020 | 
| Amazon Cognito Pinpoint document updates |  New service\-linked role was added\. Instructions were updated on "Using Amazon Pinpoint Analytics with Amazon Cognito User Pools"\.  | May 13, 2020 | 
| Amazon Cognito supported in US AWS GovCLoud |  Amazon Cognito is now supported in the AWS GovCloud \(US\) Region\.  | May 13, 2020 | 
| New Amazon Cognito dedicated security chapter |  The Security chapter can help your organization get in\-depth information about both the built\-in and the configurable security of AWS services\. Our new chapters provide information about the security of the cloud and in the cloud\.  | April 30, 2020 | 
| Amazon Cognito Identity Pools now supports Sign in with Apple |  Sign in with Apple is available in all regions where Amazon Cognito operates, except cn\-north\-1 region\.  | April 7, 2020 | 
| New Facebook API Versioning | Added version selection to Facebook API\.  | April 3, 2020 | 
| Username case insensitivity update | Added recommendation about enabling [username case insensitivity](cognito-user-pool-as-user-directory.md) before creating a user pool\. | February 11, 2020 | 
| New information about AWS Amplify | Added information about [integrating Amazon Cognito](cognito-integrate-apps.md) with your web or mobile app by using AWS Amplify SDKs and libraries\. Removed information about using the Amazon Cognito SDKs that preceded AWS Amplify\. | November 22, 2019 | 
| New attribute for user pool triggers | Amazon Cognito now includes a clientMetadata parameter in the event information that it passes to the AWS Lambda functions for most user pool triggers\. You can use this parameter to enhance your custom authentication workflow with additional data\. | October 4, 2019 | 
| Updated limit | The throttling limit for the ListUsers API action is updated\. For more information, see [Quotas in Amazon Cognito](limits.md)\. | June 25, 2019 | 
| New limit | The soft limits for user pools now include a limit for the number of users\. For more information, see [Quotas in Amazon Cognito](limits.md)\. | June 17, 2019 | 
| Amazon SES email settings for Amazon Cognito user pools | You can configure a user pool so that Amazon Cognito emails your users by using your Amazon SES configuration\. This setting allows Amazon Cognito to send email with a higher delivery volume than is otherwise possible\. For more information, see [Email Settings for Amazon Cognito User Pools](user-pool-email.md)\. | April 8, 2019 | 
| Tagging support | Added information about [tagging Amazon Cognito resources](tagging.md)\. | March 26, 2019 | 
| Change the certificate for a custom domain | If you use a custom domain to host the Amazon Cognito hosted UI, you can change the SSL certificate for this domain as needed\. For more information, see [Changing the SSL Certificate for Your Custom Domain](cognito-user-pools-add-custom-domain.md#cognito-user-pools-add-custom-domain-changing-certificate)\. | December 19, 2018 | 
| New limit | A new limit is added for the maximum number of groups that each user can belong to\. For more information, see [Quotas in Amazon Cognito](limits.md)\. | December 14, 2018 | 
| Updated limits | The soft limits for user pools are updated\. For more information, see [Quotas in Amazon Cognito](limits.md)\. | December 11, 2018 | 
| Documentation update for verifying email addresses and phone numbers | Added information about configuring your user pool to require email or phone verification when a user signs up in your app\. For more information, see [Verifying Contact Information at Sign\-Up](signing-up-users-in-your-app.md#allowing-users-to-sign-up-and-confirm-themselves)\. | November 20, 2018 | 
| Documentation update for testing emails | Added guidance for initiating emails from Amazon Cognito while you test your app\. For more information, see [Sending Emails While Testing Your App](signing-up-users-in-your-app.md#managing-users-accounts-email-testing)\. | November 13, 2018 | 
| Amazon Cognito Advanced Security | Added new security features to enable developers to protect their apps and users from malicious bots, secure user accounts against compromised credentials, and automatically adjust the challenges required to sign in based on the calculated risk of the sign in attempt\. | June 14, 2018 | 
| Custom Domains for Amazon Cognito Hosted UI | Allow developers to use their own fully custom domain for the hosted UI in Amazon Cognito User Pools\. | June 4, 2018 | 
| Amazon Cognito User Pools OIDC Identity Provider | Added user pool sign\-in through an OpenID Connect \(OIDC\) identity provider such as Salesforce or Ping Identity\. | May 17, 2018 | 
| Amazon Cognito Developer Guide Update | Added top level "What is Amazon Cognito" and "Getting Started with Amazon Cognito"\. Also added common scenarios and reorganized the user pools TOC\. Added a new "Getting Started with Amazon Cognito user pools" section\. | April 6, 2018 | 
| Amazon Cognito Lambda Migration Trigger | Added pages covering the Lambda Migration Trigger feature | February 8, 2018 | 
| Amazon Cognito Advanced Security Beta | Added new security features to enable developers to protect their apps and users from malicious bots, secure user accounts against credentials in the wild that have been compromised elsewhere on the internet, and automatically adjust the challenges required to sign in based on the calculated risk of the sign in attempt\. | November 28, 2017 | 
| Amazon Pinpoint integration | Added the ability to use Amazon Pinpoint to provide analytics for your Amazon Cognito User Pools apps and to enrich the user data for Amazon Pinpoint campaigns\. For more information, see [Using Amazon Pinpoint Analytics with Amazon Cognito User Pools](cognito-user-pools-pinpoint-integration.md)\. | September 26, 2017 | 
| Federation and built\-in app UI features of Amazon Cognito User Pools | Added the ability to allow your users to sign in to your user pool through Facebook, Google, Login with Amazon, or a SAML identity provider\. Added a customizable built\-in app UI and OAuth 2\.0 support with custom claims\. | August 10, 2017 | 
| HIPAA and PCI compliance\-related feature changes | Added the ability to allow your users to use a phone number or email address as their user name\. | July 6, 2017 | 
| User groups and role\-based access control features | Added administrative capability to create and manage user groups\. Administrators can assign IAM roles to users based on group membership and administrator\-created rules\. For more information, see [Adding Groups to a User Pool](cognito-user-pools-user-groups.md) and [Role\-Based Access Control](role-based-access-control.md)\. | December 15, 2016 | 
| Documentation update | Updated iOS code examples in [Developer Authenticated Identities \(Identity Pools\)](developer-authenticated-identities.md)\. | November 18, 2016 | 
| Documentation update | Added information about confirmation flow for user accounts\. For more information, see [Signing Up and Confirming User Accounts](signing-up-users-in-your-app.md)\. | November 9, 2016 | 
| Create user accounts feature | Added administrative capability to create user accounts through the Amazon Cognito console and the API\. For more information, see [Creating User Accounts as Administrator](how-to-create-user-accounts.md)\. | October 6, 2016 | 
| Documentation update  | Updated examples that show how to use AWS Lambda triggers with user pools\. For more information, see [Customizing User Pool Workflows with Lambda Triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\. | September 27, 2016 | 
| User import feature | Added bulk import capability for Cognito User Pools\. Use this feature to migrate users from your existing identity provider to an Amazon Cognito user pool\. For more information, see [Importing Users into User Pools From a CSV File](cognito-user-pools-using-import-tool.md)\. | September 1, 2016 | 
| General availability of Cognito User Pools  | Added the Cognito User Pools feature\. Use this feature to create and maintain a user directory and add sign\-up and sign\-in to your mobile app or web application using user pools\. For more information, see [Amazon Cognito user pools](cognito-user-identity-pools.md)\. | July 28, 2016 | 
| SAML support | Added support for authentication with identity providers through Security Assertion Markup Language 2\.0 \(SAML 2\.0\)\. For more information, see [SAML Identity Providers \(Identity Pools\)](saml-identity-provider.md)\.  | June 23, 2016 | 
| CloudTrail integration | Added integration with AWS CloudTrail\. For more information, see [Logging Amazon Cognito API Calls with AWS CloudTrail](logging-using-cloudtrail.md)\.  | February 18, 2016 | 
| Integration of events with Lambda | Enables you to execute an AWS Lambda function in response to important events in Amazon Cognito\. For more information, see [Amazon Cognito Events](cognito-events.md)\. | April 9, 2015 | 
| Data stream to Amazon Kinesis | Provides control and insight into your data streams\. For more information, see [Amazon Cognito Streams](cognito-streams.md)\. | March 4, 2015 | 
| Push synchronization | Enables support for silent push synchronization\. For more information, see [Amazon Cognito Sync](cognito-sync.md)\.  | November 6, 2014 | 
| OpenID Connect support | Enables support for OpenID Connect providers\. For more information, see [Identity Pools \(Federated Identities\) External Identity Providers](external-identity-providers.md)\. | October 23, 2014 | 
| Developer\-authenticated identities support added | Enables developers who own their own authentication and identity management systems to be treated as an identity provider in Amazon Cognito\. For more information, see [Developer Authenticated Identities \(Identity Pools\)](developer-authenticated-identities.md)\. | September 29, 2014 | 
| Amazon Cognito general availability |  | July 10, 2014 | 