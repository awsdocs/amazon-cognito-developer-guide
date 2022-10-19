# Signing up and signing in with the hosted UI<a name="cognito-user-pools-hosted-ui-user-experience"></a>

After you configure and customize the Amazon Cognito hosted UI for your user pool and app clients, your app can present it to your users\. The hosted UI supports several Amazon Cognito authentication operations, including the following examples\.
+ Sign up as a new user in your app
+ Verify an email address or phone number
+ Set up multi\-factor authentication \(MFA\)
+ Sign in with a native user name and password
+ Sign in with a third\-party identity provider \(IdP\)
+ Reset a password

The Amazon Cognito hosted UI begins at the [Login endpoint](authorization-endpoint.md)\. The URL to your sign\-in page is a combination of the domain that you chose for your user pool, and parameters that reflect the OAuth 2\.0 grants that you wish to issue, your app client, the path to your app, and the OpenID Connect \(OIDC\) scopes that you want to request\.

```
https://<your user pool domain>/authorize?client_id=<your app client ID>&response_type=<code/token>&scope=<scopes to request>&redirect_uri=<your callback URL>
```

The following URL replaces the placeholder fields above with example values\.

```
https://auth.example.com/authorize? /
client_id=1example23456789 /
&response_type=code /
&scope=aws.cognito.signin.user.admin+email+openid+profile /
&redirect_uri=https%3A%2F%2Faws.amazon.com
```

The sign\-in page for the Amazon Cognito hosted UI has options to sign in through the user pool or any identity providers \(IdPs\) that you assigned to the app client that your user is requesting\. It also includes links to sign up for a new user account in the user pool or to reset a forgotten password\.

![\[hosted UI sign-in page\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[hosted UI sign-in page\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Topics**
+ [How to sign up for a new account in the Amazon Cognito hosted UI](cognito-user-pools-hosted-ui-user-sign-up.md)
+ [How to sign in with the Amazon Cognito hosted UI](cognito-user-pools-hosted-ui-user-sign-in.md)
+ [How to reset a password with the Amazon Cognito hosted UI](cognito-user-pools-hosted-ui-user-forgot-password.md)