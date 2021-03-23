# TOTP Software Token MFA<a name="user-pool-settings-mfa-totp"></a>

Your user is challenged to complete authentication using a time\-based one\-time \(TOTP\) password\. This option is enabled after the user name and password have been verified during activation of the TOTP software token for MFA\. If your app is using the Amazon Cognito hosted UI to sign in users, the UI shows a second page\. This page asks your user to submit their user name and password and then enter the TOTP password\. 

You can enable TOTP MFA for your user pool in the Amazon Cognito console, through the Amazon Cognito hosted UI, or using Amazon Cognito API operations\. At the user pool level, you can configure MFA and enable TOTP MFA by calling [SetUserPoolMfaConfig](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUserPoolMfaConfig.html)\.

**Note**  
If TOTP software token MFA isn't enabled for the user pool, users can't be associated or verified with the token\. They receive this `SoftwareTokenMFANotFoundException` exception: "Software Token MFA has not been enabled by the userPool\." Users who have previously associated and verified a TOTP token can continue to use it for MFA if the software token MFA is later disabled for the user pool\.

Configuring TOTP for your user is a multi\-step process where your user receives a secret code that they validate by entering a one\-time password\. Next, you can enable TOTP MFA for your user or set TOTP as the preferred MFA method for your user\. 

To add MFA to your user pool, see [Adding Multi\-Factor Authentication \(MFA\) to a User Pool](user-pool-settings-mfa.md)\.

## Associate the TOTP Token<a name="user-pool-settings-mfa-totp-associate-token"></a>

Associating the TOTP token involves sending your user a secret code that they must validate with a one\-time password\. This part has three steps\.

1. When your user chooses TOTP software token MFA, call [AssociateSoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AssociateSoftwareToken.html) to return a unique generated shared secret key code for the user account\. The request for this API method takes an access token or a session string, but not both\. As a convenience, you can distribute the secret key as a quick response \(QR\) code\. 

1. The key code or QR code appears on your app\. Your user needs to enter it into a TOTP\-generating app such as Google Authenticator\. 

1. Your user enters the key code into the TOTP\-generating app to associate a new account with your client app\. 

## Verify the TOTP Token<a name="user-pool-settings-mfa-totp-verification"></a>

The next step is to verify the TOTP token\. Here is an overview of the process\.

1. After a new TOTP account is associated with your app, it generates a temporary password\.

1. Your user enters the temporary password into your app, which responds with a call to [VerifySoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifySoftwareToken.html)\. On the Amazon Cognito service server, a TOTP code is generated and compared with your user's temporary password\. If they match, then the service marks it as verified\. 

1. If the code is correct, check that the time used is in the range and within the maximum number of retries\. Amazon Cognito also accepts TOTP tokens that are one 30\-second window early or late to account for clock skew\. If your user passes all of the steps, the verification is complete\. 

   If the code is wrong, the verification cannot be finished and your user can either try again or cancel\. We recommend that your user sync the time of their TOTP\-generating app\.

## Sign in with TOTP MFA<a name="user-pool-settings-mfa-totp-sign-in"></a>

At this point, your user signs in with the time\-based one\-time password\. The process is as follows\.

1. Your user enters their user name and password to sign in to your client app\. 

1. The TOTP MFA challenge is invoked, and your user is prompted by your app to enter a temporary password\.

1. Your user gets the temporary password from an associated TOTP\-generating app\.

1. Your user enters the TOTP code into your client app\. Your app notifies the Amazon Cognito service to verify it\. For each sign\-in, [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) should be called to get a response to the new TOTP authentication challenge\.

1. If the token is verified by Amazon Cognito, the sign\-in is successful and your user continues with the authentication flow\. 

## Remove the TOTP Token<a name="user-pool-settings-mfa-totp-remove"></a>

Finally, your app should allow your user to remove the TOTP token:

1. Your client app should ask your user to enter their password\.

1. If the password is correct, remove the TOTP token\. 
**Note**  
A delete TOTP software token operation is not currently available in the API\. This functionality is planned for a future release\. Use [SetUserMFAPreference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUserMFAPreference.html) to disable TOTP MFA for an individual user\.