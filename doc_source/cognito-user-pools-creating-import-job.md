# Creating and running the Amazon Cognito user pool import job<a name="cognito-user-pools-creating-import-job"></a>

This section describes how to create and run the user pool import job by using the Amazon Cognito console and the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [Importing users from a CSV file \(console\)](#cognito-user-pools-using-import-tool-console)
+ [Importing users \(AWS CLI\)](#cognito-user-pools-using-import-tool-cli)

## Importing users from a CSV file \(console\)<a name="cognito-user-pools-using-import-tool-console"></a>

The following procedure describes how to import the users from the CSV file\.

------
#### [ Original console ]

**To import users from the CSV file \(console\)**

1. Choose **Create import job**\.

1. Enter a **Job name**\. Job names can contain uppercase and lowercase letters \(a\-z, A\-Z\), numbers \(0\-9\), and the following special characters: \+ = , \. @ and \-\.

1. If this is your first time creating a user import job, the AWS Management Console will automatically create an IAM role for you\. Otherwise, you can choose an existing role from the **IAM Role** list or let the AWS Management Console create a new role for you\.

1. Choose **Upload CSV** and select the CSV file to import users from\.

1. Choose **Create job**\.

1. To start the job, choose **Start**\.

------
#### [ New console ]

**To import users from the CSV file \(console\)**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Users** tab\.

1. In the **Import users** section, choose **Create an import job**\.

1. On the **Create import job** page, enter a **Job name**\.

1. Choose to **Create a new IAM role** or to **Use an existing IAM role**\.

   1. If you chose **Create a new IAM role**, enter a name for your new role\. Amazon Cognito will automatically create a role with the correct permissions and trust relationship\. The IAM principal that creates the import job must have permissions to create IAM roles\.

   1. If you chose **Use an existing IAM role**, choose a role from the list under **IAM role selection**\. This role must have the permissions and trust policy described in [Creating the CloudWatch Logs IAM role](cognito-user-pools-using-import-tool-cli-cloudwatch-iam-role.md)\.

1. Choose **Create job** to submit your job, but start it later\. Choose **Create and start job** to submit your job and start it immediately\.

1. If you created your job but didn't start it, you can start it later\. In the **Users** tab under **Import users**, choose your import job, then select **Start**\. You can also submit a [StartUserImportJob](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_StartUserImportJob.html) API request from an AWS SDK\.

1. Monitor the progress of your user import job in the **Users** tab under **Import users**\. If your job doesn't succeed, you can select the **Status** value\. For additional details, select **View the CloudWatch logs for more details** and review any issues in the CloudWatch Logs console\.

------

## Importing users \(AWS CLI\)<a name="cognito-user-pools-using-import-tool-cli"></a>

The following CLI commands are available for importing users into a user pool:
+ `create-user-import-job`
+ `get-csv-header`
+ `describe-user-import-job`
+ `list-user-import-jobs`
+ `start-user-import-job`
+ `stop-user-import-job`

To get the list of command line options for these commands, use the `help` command line option\. For example:

```
aws cognito-idp get-csv-header help
```

### Creating a user import job<a name="cognito-user-pools-using-import-tool-cli-creating-user-import-job"></a>

After you create your CSV file, create a user import job by running the following CLI command, where *JOB\_NAME* is the name you're choosing for the job, *USER\_POOL\_ID* is the user pool ID for the user pool into which the new users will be added, and *ROLE\_ARN* is the role ARN you received in [Creating the CloudWatch Logs IAM role](cognito-user-pools-using-import-tool-cli-cloudwatch-iam-role.md): 

```
aws cognito-idp create-user-import-job --job-name "JOB_NAME" --user-pool-id "USER_POOL_ID" --cloud-watch-logs-role-arn "ROLE_ARN"
```

The *PRE\_SIGNED\_URL* returned in the response is valid for 15 minutes\. After that time, it will expire and you must create a new user import job to get a new URL\.

**Example Sample response:**  

```
{
    "UserImportJob": {
        "Status": "Created",
        "SkippedUsers": 0,
        "UserPoolId": "USER_POOL_ID",
        "ImportedUsers": 0,
        "JobName": "JOB_NAME",
        "JobId": "JOB_ID",
        "PreSignedUrl": "PRE_SIGNED_URL",
        "CloudWatchLogsRoleArn": "ROLE_ARN",
        "FailedUsers": 0,
        "CreationDate": 1470957431.965
    }
}
```

### Status values for a user import job<a name="cognito-user-pools-using-import-tool-cli-status-values-for-user-import-job"></a>

In the responses to your user import commands, you'll see one of the following `Status` values:
+ `Created` \- The job was created but not started\.
+ `Pending` \- A transition state\. You have started the job, but it has not begun importing users yet\.
+ `InProgress` \- The job has started, and users are being imported\.
+ `Stopping` \- You have stopped the job, but the job has not stopped importing users yet\.
+ `Stopped` \- You have stopped the job, and the job has stopped importing users\.
+ `Succeeded` \- The job has completed successfully\.
+ `Failed` \- The job has stopped due to an error\.
+ `Expired` \- You created a job, but did not start the job within 24\-48 hours\. All data associated with the job was deleted, and the job can't be started\.

### Uploading the CSV file<a name="cognito-user-pools-using-import-tool-cli-uploading-csv-file"></a>

Use the following `curl` command to upload the CSV file containing your user data to the presigned URL that you obtained from the response of the `create-user-import-job` command\.

```
curl -v -T "PATH_TO_CSV_FILE" -H "x-amz-server-side-encryption:aws:kms" "PRE_SIGNED_URL"
```

In the output of this command, look for the phrase `"We are completely uploaded and fine"`\. This phrase indicates that the file was uploaded successfully\.

### Describing a user import job<a name="cognito-user-pools-using-import-tool-cli-describing-user-import-job"></a>

To get a description of your user import job, use the following command, where *USER\_POOL\_ID* is your user pool ID, and *JOB\_ID* is the job ID that was returned when you created the user import job\. 

```
aws cognito-idp describe-user-import-job --user-pool-id "USER_POOL_ID" --job-id "JOB_ID"
```

**Example Sample response:**  

```
{
    "UserImportJob": {
        "Status": "Created",
        "SkippedUsers": 0,
        "UserPoolId": "USER_POOL_ID",
        "ImportedUsers": 0,
        "JobName": "JOB_NAME",
        "JobId": "JOB_ID",
        "PreSignedUrl": "PRE_SIGNED_URL",
        "CloudWatchLogsRoleArn":"ROLE_ARN",
        "FailedUsers": 0,
        "CreationDate": 1470957431.965
    }
}
```

In the preceding sample output, the *PRE\_SIGNED\_URL* is the URL that you uploaded the CSV file to\. The *ROLE\_ARN* is the CloudWatch Logs role ARN that you received when you created the role\.

### Listing your user import jobs<a name="cognito-user-pools-using-import-tool-cli-listing-user-import-jobs"></a>

To list your user import jobs, use the following command:

```
aws cognito-idp list-user-import-jobs --user-pool-id "USER_POOL_ID" --max-results 2
```

**Example Sample response:**  

```
{
    "UserImportJobs": [
        {
            "Status": "Created",
            "SkippedUsers": 0,
            "UserPoolId": "USER_POOL_ID",
            "ImportedUsers": 0,
            "JobName": "JOB_NAME",
            "JobId": "JOB_ID",
            "PreSignedUrl":"PRE_SIGNED_URL",
            "CloudWatchLogsRoleArn":"ROLE_ARN",
            "FailedUsers": 0,
            "CreationDate": 1470957431.965
        },
        {
            "CompletionDate": 1470954227.701,
            "StartDate": 1470954226.086,
            "Status": "Failed",
            "UserPoolId": "USER_POOL_ID",
            "ImportedUsers": 0,
            "SkippedUsers": 0,
            "JobName": "JOB_NAME",
            "CompletionMessage": "Too many users have failed or been skipped during the import.",
            "JobId": "JOB_ID",
            "PreSignedUrl":"PRE_SIGNED_URL",
            "CloudWatchLogsRoleArn":"ROLE_ARN",
            "FailedUsers": 5,
            "CreationDate": 1470953929.313
        }
    ],
    "PaginationToken": "PAGINATION_TOKEN"
}
```

Jobs are listed in chronological order from last created to first created\. The *PAGINATION\_TOKEN* string after the second job indicates that there are additional results for this list command\. To list the additional results, use the `--pagination-token` option as follows:

```
aws cognito-idp list-user-import-jobs --user-pool-id "USER_POOL_ID" --max-results 10 --pagination-token "PAGINATION_TOKEN"
```

### Starting a user import job<a name="cognito-user-pools-using-import-tool-cli-starting-user-import-job"></a>

To start a user import job, use the following command:

```
aws cognito-idp start-user-import-job --user-pool-id "USER_POOL_ID" --job-id "JOB_ID"
```

Only one import job can be active at a time per account\.

**Example Sample response:**  

```
{
    "UserImportJob": {
        "Status": "Pending",
        "StartDate": 1470957851.483,
        "UserPoolId": "USER_POOL_ID",
        "ImportedUsers": 0,
        "SkippedUsers": 0,
        "JobName": "JOB_NAME",
        "JobId": "JOB_ID",
        "PreSignedUrl":"PRE_SIGNED_URL",
        "CloudWatchLogsRoleArn": "ROLE_ARN",
        "FailedUsers": 0,
        "CreationDate": 1470957431.965
    }
}
```

### Stopping a user import job<a name="cognito-user-pools-using-import-tool-cli-stopping-user-import-job"></a>

To stop a user import job while it is in progress, use the following command\. After you stop the job, it cannot be restarted\.

```
aws cognito-idp stop-user-import-job --user-pool-id "USER_POOL_ID" --job-id "JOB_ID"
```

**Example Sample response:**  

```
{
    "UserImportJob": {
        "CompletionDate": 1470958050.571,
        "StartDate": 1470958047.797,
        "Status": "Stopped",
        "UserPoolId": "USER_POOL_ID",
        "ImportedUsers": 0,
        "SkippedUsers": 0,
        "JobName": "JOB_NAME",
        "CompletionMessage": "The Import Job was stopped by the developer.",
        "JobId": "JOB_ID",
        "PreSignedUrl":"PRE_SIGNED_URL",
        "CloudWatchLogsRoleArn": "ROLE_ARN",
        "FailedUsers": 0,
        "CreationDate": 1470957972.387
    }
}
```