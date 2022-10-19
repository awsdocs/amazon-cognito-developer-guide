# TOTP software token MFA<a name="user-pool-settings-mfa-totp"></a>

When you set up TOTP software token MFA in your user pool, your user signs in with a user name and password, then uses a TOTP to complete authentication\. After your user sets and verifies a user name and password, they can activate a TOTP software token for MFA\. If your app uses the Amazon Cognito hosted UI to sign in users, your user submits their user name and password, and then submits the TOTP password on an additional sign\-in page\.

You can activate TOTP MFA for your user pool in the Amazon Cognito console, or you can use Amazon Cognito API operations\. At the user pool level, you can call [SetUserPoolMfaConfig](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUserPoolMfaConfig.html) to configure MFA and enable TOTP MFA\.

**Note**  
If you haven't activated TOTP software token MFA for the user pool, Amazon Cognito can't use the token to associate or verify users\. In this case, users receive a `SoftwareTokenMFANotFoundException` exception with the description `Software Token MFA has not been enabled by the userPool`\. If you deactivate software token MFA for the user pool later, users who previously associated and verified a TOTP token can continue to use it for MFA\.

Configuring TOTP for your user is a multi\-step process where your user receives a secret code that they validate by entering a one\-time password\. Next, you can enable TOTP MFA for your user or set TOTP as the preferred MFA method for your user\.

When you configure your user pool to require TOTP MFA and your users sign up for your app in the hosted UI, Amazon Cognito automates the user process\. Amazon Cognito prompts your user to choose an MFA method, displays a QR code to set up their authenticator app, and verifies their MFA registration\. In user pools where you have allowed users to choose between SMS and TOTP MFA, Amazon Cognito also presents your user with a choice of method\. For more information about the hosted UI sign\-up experience, see [How to sign up for a new account in the Amazon Cognito hosted UI](cognito-user-pools-hosted-ui-user-sign-up.md)\.

**Important**  
When you have an AWS WAF web ACL associated with a user pool, and a rule in your web ACL presents a CAPTCHA, this can cause an unrecoverable error in hosted UI TOTP registration\. To create a rule that has a CAPTCHA action and doesn't affect hosted UI TOTP, see [Configuring your AWS WAF web ACL for hosted UI TOTP MFA](#hosted-ui-totp-waf)\. For more information about AWS WAF web ACLs and Amazon Cognito, see [Associating an AWS WAF web ACL with a user pool](user-pool-waf.md)\.

To implement TOTP MFA in a custom UI where you use the [Amazon Cognito API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/Welcome.html), see [Configuring MFA for a user in the Amazon Cognito native API](#totp-mfa-set-up-api)\.

To add MFA to your user pool, see [Adding MFA to a user pool](user-pool-settings-mfa.md)\.

**TOTP MFA considerations and limitations**

1. Amazon Cognito supports software token MFA through an authenticator app that generates TOTP codes\. Amazon Cognito doesn't support hardware\-based MFA\.

1. When your user pool requires TOTP for a user who has not configured it, your user receives a one\-time access token that your app can use to activate TOTP MFA for the user\. Subsequent sign\-in attempts fail until your user has registered an additional TOTP sign\-in factor\.
   + A user who signs up in your user pool with the `SignUp` API operation or through the hosted UI receives one\-time tokens when the user completes sign\-up\.
   + After you create a user, and the user sets their initial password, Amazon Cognito issues one\-time tokens from the hosted UI to the user\. If you set a permanent password for the user, Amazon Cognito issues one\-time tokens when the user first signs in\.
   + Amazon Cognito doesn't issue one\-time tokens to an administrator\-created user who signs in with the [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) or [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) API operations\. After your user succeeds in the challenge to set their initial password, or if you set a permanent password for the user, Amazon Cognito immediately challenges the user to set up MFA\.

1. If a user in a user pool that requires MFA has already received a one\-time access token but hasn't set up TOTP MFA, the user can't sign in with the hosted UI until they have set up MFA\. Instead of the access token, you can use the `session` response value from an `MFA_SETUP` challenge to [InitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_InitiateAuth.html) or [AdminInitiateAuth](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminInitiateAuth.html) in an [AssociateSoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AssociateSoftwareToken.html) request\.

1. If your users have set up TOTP, they can use it for MFA, even if you deactivate TOTP for the user pool later\.

## Configuring MFA for a user in the Amazon Cognito native API<a name="totp-mfa-set-up-api"></a>

When a user first signs in, your app uses their one\-time access token to generate the TOTP private key and present it to your user in text or QR code format\. Your user configures their authenticator app and provides a TOTP for subsequent sign\-in attempts\. Your app or the hosted UI presents the TOTP to Amazon Cognito in MFA challenge responses\.

**Topics**
+ [Associate the TOTP software token](#user-pool-settings-mfa-totp-associate-token)
+ [Verify the TOTP token](#user-pool-settings-mfa-totp-verification)
+ [Sign in with TOTP MFA](#user-pool-settings-mfa-totp-sign-in)
+ [Remove the TOTP token](#user-pool-settings-mfa-totp-remove)

### Associate the TOTP software token<a name="user-pool-settings-mfa-totp-associate-token"></a>

To associate the TOTP token, send your user a secret code that they must validate with a one\-time password\. Associating the token requires three steps\.

1. When your user chooses TOTP software token MFA, call [AssociateSoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AssociateSoftwareToken.html) to return a unique generated shared secret key code for the user account\. You can authorize AssociateSoftwareToken with either an access token or a session string\. 

1. Your app presents the user with the private key, or a QR code that you generate from the private key\. Your user must enter the key into a TOTP\-generating app such as Google Authenticator\. You can use [libqrencode](https://github.com/fukuchi/libqrencode/) to generate a QR code\.

1. Your user enters the key, or scans the QR code into a authenticator app such as Google Authenticator, and the app begins generating codes\.

### Verify the TOTP token<a name="user-pool-settings-mfa-totp-verification"></a>

Next, verify the TOTP token\. Request sample codes from your user and provide them to the Amazon Cognito service to confirm that the user is successfully generating TOTP codes, as follows\.

1. Your app prompts your user for a code to demonstrate that they have set up their authenticator app properly\.

1. The user's authenticator app displays a temporary password\. The authenticator app bases the password on the secret key you gave to the user\.

1. Your user enters their temporary password\. Your app passes the temporary password to Amazon Cognito in a `[VerifySoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifySoftwareToken.html)` API request\.

1.  Amazon Cognito has retained the secret key associated with the user, and generates a TOTP and compares it with the one that your user provided\. If they match, `VerifySoftwareToken` returns a `SUCCESS` response\.

1. Amazon Cognito associates the TOTP factor with the user\.

1. If the `VerifySoftwareToken` operation returns an `ERROR` response, make sure that the user's clock is correct and that they have not exceeded the maximum number of retries\. Amazon Cognito accepts TOTP tokens that are within 30 seconds before or after the attempt, to account for minor clock skew\. When you have resolved the issue, try the VerifySoftwareToken operation again\.

### Sign in with TOTP MFA<a name="user-pool-settings-mfa-totp-sign-in"></a>

At this point, your user signs in with the time\-based one\-time password\. The process is as follows\.

1. Your user enters their user name and password to sign in to your client app\. 

1. The TOTP MFA challenge is invoked, and your user is prompted by your app to enter a temporary password\.

1. Your user gets the temporary password from an associated TOTP\-generating app\.

1. Your user enters the TOTP code into your client app\. Your app notifies the Amazon Cognito service to verify it\. For each sign\-in, [RespondToAuthChallenge](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_RespondToAuthChallenge.html) should be called to get a response to the new TOTP authentication challenge\.

1. If the token is verified by Amazon Cognito, the sign\-in is successful and your user continues with the authentication flow\. 

### Remove the TOTP token<a name="user-pool-settings-mfa-totp-remove"></a>

Finally, your app should allow your user to remove the TOTP token:

1. Your client app should ask your user to enter their password\.

1. If the password is correct, remove the TOTP token\. 
**Note**  
Currently, you can't delete a user's TOTP software token\. To replace your user's software token, associate and verify a new software token\. To deactivate TOTP MFA for a user, call [SetUserMFAPreference](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUserMFAPreference.html) to modify your user to use no MFA, or only SMS MFA\.

## Configuring your AWS WAF web ACL for hosted UI TOTP MFA<a name="hosted-ui-totp-waf"></a>

When you have an AWS WAF web ACL associated with a user pool, and a rule in your web ACL presents a CAPTCHA, this can cause an unrecoverable error in hosted UI TOTP registration\. AWS WAF CAPTCHA rules only affect TOTP MFA in the hosted UI in this way\. SMS MFA is unaffected\. 

Amazon Cognito displays the following error when your CAPTCHA rule doesn't let a user complete TOTP MFA setup\. 

Request not allowed due to WAF captcha\.

This error results when AWS WAF prompts for a CAPTCHA in response to [AssociateSoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AssociateSoftwareToken.html) and [VerifySoftwareToken](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_VerifySoftwareToken.html) API requests that your user pool makes in the background\. To create a rule that has a CAPTCHA action and doesn't affect hosted UI TOTP, exclude the `x-amzn-cognito-operation-name` header values of `AssociateSoftwareToken` and `VerifySoftwareToken` from the CAPTCHA action in your rule\.

The following screenshot shows an example AWS WAF rule that applies a CAPTCHA action to all requests that don't have a `x-amzn-cognito-operation-name` header value of `AssociateSoftwareToken` or `VerifySoftwareToken`\.

![\[A screenshot of a AWS WAF rule that applies a CAPTCHA action to all requests that don't have a x-amzn-cognito-operation-name header value of AssociateSoftwareToken or VerifySoftwareToken.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[A screenshot of a AWS WAF rule that applies a CAPTCHA action to all requests that don't have a x-amzn-cognito-operation-name header value of AssociateSoftwareToken or VerifySoftwareToken.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[A screenshot of a AWS WAF rule that applies a CAPTCHA action to all requests that don't have a x-amzn-cognito-operation-name header value of AssociateSoftwareToken or VerifySoftwareToken.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

For more information about AWS WAF web ACLs and Amazon Cognito, see [Associating an AWS WAF web ACL with a user pool](user-pool-waf.md)\.