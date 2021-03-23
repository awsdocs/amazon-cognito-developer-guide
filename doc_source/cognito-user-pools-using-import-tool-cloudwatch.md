# Viewing the User Pool Import Results in the CloudWatch Console<a name="cognito-user-pools-using-import-tool-cloudwatch"></a>

You can view the results of your import job in the Amazon CloudWatch console\.

**Topics**
+ [Viewing the Results](#cognito-user-pools-using-import-tool-viewing-the-results)
+ [Interpreting the Results](#cognito-user-pools-using-import-tool-interpreting-the-results)

## Viewing the Results<a name="cognito-user-pools-using-import-tool-viewing-the-results"></a>

The following steps describe how to view the user pool import results\.

**To view the results of the user pool import**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Logs**\.

1. Choose the log group for your user pool import jobs\. The log group name is in the form `/aws/cognito/userpools/USER_POOL_ID/USER_POOL_NAME`\.

1. Choose the log for the user import job you just ran\. The log name is in the form *JOB\_ID*/*JOB\_NAME*\. The results in the log refer to your users by line number\. No user data is written to the log\. For each user, a line similar to the following appears:
   + `[SUCCEEDED] Line Number 5956 - The import succeeded.`
   + `[SKIPPED] Line Number 5956 - The user already exists.`
   + `[FAILED] Line Number 5956 - The User Record does not set any of the auto verified attributes to true. (Example: email_verified to true).`

## Interpreting the Results<a name="cognito-user-pools-using-import-tool-interpreting-the-results"></a>

Successfully imported users have their status set to "PasswordReset"\.

In the following cases, the user will not be imported, but the import job will continue:
+ No auto\-verified attributes are set to `true`\.
+ The user data doesn't match the schema\.
+ The user couldn't be imported due to an internal error\.

In the following cases, the import job will fail:
+ The Amazon CloudWatch Logs role cannot be assumed, doesn't have the correct access policy, or has been deleted\.
+ The user pool has been deleted\.
+ Amazon Cognito is unable to parse the \.csv file\.