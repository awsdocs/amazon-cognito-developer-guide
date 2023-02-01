# Linking federated users to an existing user profile<a name="cognito-user-pools-identity-federation-consolidate-users"></a>

Often, the same user has a profile with multiple identity providers \(IdPs\) that you have connected to your user pool\. Amazon Cognito can link each occurrence of a user to the same user profile in your directory\. This way, one person who has multiple IdP users can have a consistent experience in your app\. [AdminLinkProviderForUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminLinkProviderForUser.html) tells Amazon Cognito to recognize a user's unique ID in your federated directory as a user in the user pool\. A user in your user pool counts as one monthly active user \(MAU\) for the purposes of [billing](http://aws.amazon.com/cognito/pricing/) when you have zero or more federated identities associated with the user profile\.

When a federated user signs in to a user pool, Amazon Cognito creates a user profile that it prefixes with the name of your IdP\. You can find both the linked native user and the automatically\-created federated user when you search users in your user pool\. In their tokens, your linked user's identity claims originate from their federated profile\. Their access claims originate from their native profile\.

**Important**  
Because `AdminLinkProviderForUser` allows a user with an external federated identity to sign in as an existing user in the user pool, it is critical that it only be used with external IdPs and provider attributes that have been trusted by the application owner\.

For example, if you're a managed service provider \(MSP\) with an app that you share with multiple customers\. Each of the customers signs in to your app through Active Directory Federation Services \(ADFS\)\. Your IT administrator, Carlos, has an account in each of your customers' domains\. You want Carlos to be recognized as an app administrator every time he signs in, regardless of the IdP\.

Your ADFS IdPs present Carlos' email address `msp_carlos@example.com` in the `email` claim of the Carlos' SAML assertions to Amazon Cognito\. You create a user in your user pool with the user name `Carlos`\. The following AWS Command Line Interface \(AWS CLI\) commands link Carlos' identities from IdPs ADFS1, ADFS2, and ADFS3\.

**Note**  
You can link a user based on specific attribute claims\. This ability is unique to SAML IdPs\. For other provider types, you must link based on a fixed source attribute\. For more information, see [AdminLinkProviderForUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminLinkProviderForUser.html)\. You must set `ProviderAttributeName` to `Cognito_Subject` when you link an OIDC or social IdP to a user profile\. `ProviderAttributeValue` must be the user's unique identifier with your IdP\.

```
aws cognito-idp admin-link-provider-for-user \
--user-pool-id us-east-1_EXAMPLE \
--destination-user ProviderAttributeValue=Carlos,ProviderName=Cognito \
--source-user ProviderName=ADFS1,ProviderAttributeName=email,ProviderAttributeValue=msp_carlos@example.com

aws cognito-idp admin-link-provider-for-user \
--user-pool-id us-east-1_EXAMPLE \
--destination-user ProviderAttributeValue=Carlos,ProviderName=Cognito \
--source-user ProviderName=ADFS2,ProviderAttributeName=email,ProviderAttributeValue=msp_carlos@example.com

aws cognito-idp admin-link-provider-for-user \
--user-pool-id us-east-1_EXAMPLE \
--destination-user ProviderAttributeValue=Carlos,ProviderName=Cognito \
--source-user ProviderName=ADFS3,ProviderAttributeName=email,ProviderAttributeValue=msp_carlos@example.com
```

The user profile `Carlos` in your user pool now has the following `identities` attribute\.

```
[{
    "userId": "msp_carlos@example.com",
    "providerName": "ADFS1",
    "providerType": "SAML",
    "issuer": "http://auth.example.com",
    "primary": false,
    "dateCreated": 111111111111111
}, {
    "userId": "msp_carlos@example.com",
    "providerName": "ADFS2",
    "providerType": "SAML",
    "issuer": "http://auth2.example.com",
    "primary": false,
    "dateCreated": 111111111111111
}, {
    "userId": "msp_carlos@example.com",
    "providerName": "ADFS3",
    "providerType": "SAML",
    "issuer": "http://auth3.example.com",
    "primary": false,
    "dateCreated": 111111111111111
}]
```

**Things to know about linking federated users**
+ You can link up to five federated users to each user profile\.
+ You can link federated users to either an existing federated user profile, or to a native user\.
+ You can't link providers to user profiles in the AWS Management Console\.
+ Your user's ID token contains all of their associated providers in the `identities` claim\.

See [AdminLinkProviderForUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminLinkProviderForUser.html) for additional command syntax and examples in the AWS SDKs\.