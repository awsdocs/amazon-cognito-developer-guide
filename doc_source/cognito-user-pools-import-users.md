# Importing Users into a User Pool<a name="cognito-user-pools-import-users"></a>

There are two ways you can import or migrate users from your existing user directory or user database into Amazon Cognito user pools\. You can migrate users when they sign\-in using Amazon Cognito for the first time with a user migration Lambda trigger\. With this approach, users can continue using their existing passwords and will not have to reset them after the migration to your user pool\. Alternatively, you can migrate users in bulk by uploading a CSV file containing the user profile attributes for all users\. The following sections describe both these approaches\.

**Topics**
+ [Importing Users into User Pools With a User Migration Lambda Trigger](cognito-user-pools-import-using-lambda.md)
+ [Importing Users into User Pools From a CSV File](cognito-user-pools-using-import-tool.md)