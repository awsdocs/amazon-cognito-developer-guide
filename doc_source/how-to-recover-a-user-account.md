# Recovering user accounts<a name="how-to-recover-a-user-account"></a>

The `AccountRecoverySetting` parameter enables you to customize which method a user can use to recover their password when they call the [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgotPassword.html) API\. `ForgotPassword` sends a recovery code to a verified email or a verified phone number\. When you specify `AccountRecoverySetting`, the preferred setting is chosen from the priorities defined by `AccountRecoverySetting`\.

When you define `AccountRecoverySetting` and a user has SMS MFA configured, SMS cannot be used as an account recovery mechanism\. The priority for this setting is determined with 1 being of the highest priority\. Cognito sends a verification to only one of the specified methods\.

For example, `admin_only` is a value used when the administrator does not want the user to recover their account themselves, and would instead require them to contact the administrator to reset their account\. You cannot use `admin_only` with any other account recovery mechanism\.

If you do not specify `AccountRecoverySetting`, Amazon Cognito uses the legacy mechanism to determine the password recovery method\. In this case, Cognito uses a verified phone first\. If the verified phone is not found for the user, Cognito falls back and will use verified email next\.

For more information about `AccountRecoverySetting`, see [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) and [UpdateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserPool.html) in the *Amazon Cognito Identity Provider API Reference*\.

## Forgot password behavior<a name="forgot-password"></a>

In a given hour, we allow between 5 and 20 attempts for a user to request or enter a password reset code as part of forgot\-password and confirm\-forgot\-password actions\. The exact value depends on the risk parameters associated with the requests\. Please note that this behavior is subject to change\. 