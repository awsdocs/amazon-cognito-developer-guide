# Importing users into user pools from a CSV file<a name="cognito-user-pools-using-import-tool"></a>

You can import users into an Amazon Cognito user pool\. The user information is imported from a specially formatted \.csv file\. The import process sets values for all user attributes except **password**\. Password import is not supported, because security best practices require that passwords are not available as plain text, and we don't support importing hashes\. This means that your users must change their passwords the first time they sign in\. So, your users will be in a RESET\_REQUIRED state when imported using this method\.

**Note**  
The creation date for each user is the time when that user was imported into the user pool\. Creation date is not one of the imported attributes\.

The basic steps are:

1. Create an Amazon CloudWatch Logs role in the AWS Identity and Access Management \(IAM\) console\.

1. Create the user import \.csv file\.

1. Create and run the user import job\.

1. Upload the user import \.csv file\.

1. Start and run the user import job\.

1. Use CloudWatch to check the event log\.

1. Require the imported users to reset their passwords\.

**Topics**
+ [Creating the CloudWatch Logs IAM role \(AWS CLI, API\)](cognito-user-pools-using-import-tool-cli-cloudwatch-iam-role.md)
+ [Creating the user import \.csv file](cognito-user-pools-using-import-tool-csv-header.md)
+ [Creating and running the Amazon Cognito user pool import job](cognito-user-pools-creating-import-job.md)
+ [Viewing the user pool import results in the CloudWatch console](cognito-user-pools-using-import-tool-cloudwatch.md)
+ [Requiring imported users to reset their passwords](cognito-user-pools-using-import-tool-password-reset.md)