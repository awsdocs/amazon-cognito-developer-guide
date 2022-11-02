# SAML identity providers \(identity pools\)<a name="saml-identity-provider"></a>

Amazon Cognito supports authentication with identity providers \(IdPs\) through Security Assertion Markup Language 2\.0 \(SAML 2\.0\)\. You can use an IdP that supports SAML with Amazon Cognito to provide a simple onboarding flow for your users\. Your SAML\-supporting IdP specifies the IAM roles that your users can assume\. This way, different users can receive different sets of permissions\.

## Configuring your identity pool for a SAML IdP<a name="configure-identity-pool-saml-provider"></a>

The following steps describe how to configure your identity pool to use a SAML\-based IdP\.

**Note**  
Before you configure your identity pool to support a SAML provider, first configure the SAML IdP in the [IAM console](https://console.aws.amazon.com/iam)\. For more information, see [Integrating third\-party SAML solution providers with AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml_3rd-party.html) in the *IAM User Guide*\.

**Configuring your identity pool to support a SAML provider**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), choose **Manage Identity Pools**, and choose **Create new identity pool**\.

1. In the **Authentication providers** section, choose the **SAML** tab\.

1. Choose the ARN of the SAML provider, and then choose **Create Pool**\.

## Configuring your SAML IdP<a name="configure-your-saml-identity-provider"></a>

After you create the SAML provider, configure your SAML IdP to add relying party trust between your IdP and AWS\. With many IdPs, you can specify a URL that the IdP can use to read relying party information and certificates from an XML document\. For AWS, you can use [https://signin\.aws\.amazon\.com/static/saml\-metadata\.xml](https://signin.aws.amazon.com/static/saml-metadata.xml)\. The next step is to configure the SAML assertion response from your IdP to populate the claims that AWS needs\. For details on the claim configuration, see [Configuring SAML assertions for authentication response](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html)\.

## Customizing your user role with SAML<a name="role-customization-saml"></a>

When you use SAML with Amazon Cognito Identity, you can customize the role for the end user\. Amazon Cognito only supports the [enhanced flow](authentication-flow.md) with the SAML\-based IdP\. You don't need to specify an authenticated or unauthenticated role for the identity pool to use a SAML\-based IdP\. The `https://aws.amazon.com/SAML/Attributes/Role` claim attribute specifies one or more pairs of comma \-delimited role and provider ARN\. These are the roles that the user can assume\. You can configure the SAML IdP to populate the role attributes based on the user attribute information available from the IdP\. If you receive multiple roles in the SAML assertion, populate the optional `customRoleArn` parameter when you call `getCredentialsForIdentity`\. The user assumes this `customRoleArn` if the role matches one in the claim in the SAML assertion\.

## Authenticating users with a SAML IdP<a name="authenticate-user-with-saml"></a>

To federate with the SAML\-based IdP, determine the URL where the user initiates the login\. AWS federation uses IdP\-initiated login\. In AD FS 2\.0 the URL takes the form of `https://<fqdn>/adfs/ls/IdpInitiatedSignOn.aspx?loginToRp=urn:amazon:webservices`\.

To add support for your SAML IdP in Amazon Cognito, first authenticate users with your SAML identity provider from your iOS or Android application\. The code that you use to integrate and authenticate with the SAML IdP is specific to SAML providers\. After you authenticate your user, you can use Amazon Cognito APIs to provide the resulting SAML assertion to Amazon Cognito Identity \.

## Android<a name="populate-saml-assertions.android"></a>

If you use the Android SDK, you can populate the logins map with the SAML assertion as follows\.

```
Map logins = new HashMap();
logins.put("arn:aws:iam::aws account id:saml-provider/name", "base64 encoded assertion response");
// Now this should be set to CognitoCachingCredentialsProvider object.
CognitoCachingCredentialsProvider credentialsProvider = new CognitoCachingCredentialsProvider(context, identity pool id, region);
credentialsProvider.setLogins(logins);
// If SAML assertion contains multiple roles, resolve the role by setting the custom role
credentialsProvider.setCustomRoleArn("arn:aws:iam::aws account id:role/customRoleName");
// This should trigger a call to the Amazon Cognito service to get the credentials.
credentialsProvider.getCredentials();
```

## iOS<a name="populate-saml-assertions.ios"></a>

If you are using the iOS SDK, you can provide the SAML assertion in `AWSIdentityProviderManager` as follows\.

```
- (AWSTask<NSDictionary<NSString*,NSString*> *> *) logins {
    //this is hardcoded for simplicity, normally you would asynchronously go to your SAML provider 
    //get the assertion and return the logins map using a AWSTaskCompletionSource
    return [AWSTask taskWithResult:@{@"arn:aws:iam::aws account id:saml-provider/name":@"base64 encoded assertion response"}];
}
 
// If SAML assertion contains multiple roles, resolve the role by setting the custom role.
// Implementing this is optional if there is only one role.
- (NSString *)customRoleArn {
    return @"arn:aws:iam::accountId:role/customRoleName";
}
```