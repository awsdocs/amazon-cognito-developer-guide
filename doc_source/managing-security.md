# Using Amazon Cognito user pools security features<a name="managing-security"></a>

You can add multi\-factor authentication \(MFA\) to a user pool to protect the identity of your users\. MFA adds a second authentication factor so that your user pool doesn't rely solely on user name and password\. You can use either SMS text messages or time\-based one\-time passwords \(TOTP\) as second factors to sign in your users\. You can also use adaptive authentication with its risk\-based model to predict when you might need another authentication factor\. User pool *advanced security* features include adaptive authentication and protections against compromised credentials\.

**Topics**
+ [Adding MFA to a user pool](user-pool-settings-mfa.md)
+ [Adding advanced security to a user pool](cognito-user-pool-settings-advanced-security.md)
+ [Associating an AWS WAF web ACL with a user pool](user-pool-waf.md)
+ [User pool case sensitivity](user-pool-case-sensitivity.md)