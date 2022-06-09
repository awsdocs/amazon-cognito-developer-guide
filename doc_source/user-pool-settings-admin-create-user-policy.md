# Configuring an admin create user policy<a name="user-pool-settings-admin-create-user-policy"></a>

You can specify the following policies for Admin Create User:
+ Specify whether to allow users to sign themselves up\. This option is set by default\. If it is not set, only administrators can create users in this pool and calls to the [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html) API fail with `NotAuthorizedException`\.
+ Specify the user account expiration time limit \(in days\) for new accounts\. The default setting is 7 days, measured from the time when an administrator or the user creates the account\. The maximum setting is 365 days\. After the account expires, the user can't log in to the account until you update the user's profile\. To do this, update an attribute or resend the password to the user\.
**Note**  
After the user logs in, the account never expires\.