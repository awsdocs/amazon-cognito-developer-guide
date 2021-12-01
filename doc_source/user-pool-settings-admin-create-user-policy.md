# Configuring an admin create user policy<a name="user-pool-settings-admin-create-user-policy"></a>

You can specify the following policies for Admin Create User:
+ Specify whether to allow users to sign themselves up\. This option is set by default\. If it is not set, only administrators can create users in this pool and calls to the [SignUp](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SignUp.html) API fail with `NotAuthorizedException`\.
+ Specify the user account expiration time limit \(in days\) for new accounts\. The default setting is 7 days, measured from the time when the user account is created\. The maximum setting is 90 days\. After the account expires, the user cannot log in to the account until the administrator updates the user's profile by updating an attribute or by resending the password to the user\.
**Note**  
Once the user has logged in, the account never expires\.