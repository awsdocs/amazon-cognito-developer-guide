# Configuring User Pool Attributes<a name="user-pool-settings-attributes"></a>

You get a set of default attributes, called "standard attributes," with all user pools\. You can also add custom attributes to your user pool definition in the AWS Management Console\. This topic describes those attributes in detail and gives you tips on how to set up your user pool\.

Attributes are pieces of information that help you identify individual users, such as name, email, and phone number\.

Not all information about your users should be stored in attributes\. For example, user data that changes frequently, such as usage statistics or game scores, should be kept in a separate data store, such as Amazon Cognito Sync or Amazon DynamoDB\.

## Standard Attributes<a name="cognito-user-pools-standard-attributes"></a>

The following are the standard attributes for all users in a user pool\. These are implemented following the [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\.
+ `address`
+ `birthdate`
+ `email`
+ `family_name`
+ `gender`
+ `given_name`
+ `locale`
+ `middle_name`
+ `name`
+ `nickname`
+ `phone_number`
+ `picture`
+ `preferred_username`
+ `profile`
+ `updated_at`
+ `website`
+ `zoneinfo`

These attributes are available as optional attributes for all users\. To make an attribute required, select the check box next to the attribute\.

**Note**  
 When a standard attribute is marked as required, a user cannot register unless a value for the attribute is provided\. Administrators can create users without giving values for required attributes by using the [AdminCreateUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminCreateUser.html) API\. An attribute cannot be switched between required and not required after a user pool has been created\. 

Standard and custom attribute values can be any string up to 2048 characters by default, but some attribute values, such as `updated_at`, have formatting restrictions\. Only **email** and **phone** can be verified\.

**Note**  
In the specification, attributes are called *members*\.

Here are some additional notes regarding some of the above fields\.

**email**  
Email address values can be verified\.  
An administrator with proper AWS account permissions can change the user's email and also mark it as verified\. This can be done by using the [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) API or the [admin\-update\-user\-attributes](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-user-attributes.html) CLI command to change the `email_verified` attribute to `true`\.

**phone**  
A phone number is required if SMS multi\-factor authentication \(MFA\) is enabled\. For more information, see [Adding Multi\-Factor Authentication \(MFA\) to a User Pool](user-pool-settings-mfa.md)\.  
Phone number values can be verified\.  
An administrator with proper AWS account permissions can change the user's phone number and also mark it as verified\. This can be done by using the [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) API or the [admin\-update\-user\-attributes](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-user-attributes.html) CLI command to change the `phone_number_verified` attribute to `true`\.  
Phone numbers must follow these formatting rules: A phone number must start with a plus \(**\+**\) sign, followed immediately by the country code\. A phone number can only contain the **\+** sign and digits\. You must remove any other characters from a phone number, such as parentheses, spaces, or dashes \(**\-**\) before submitting the value to the service\. For example, a United States\-based phone number must follow this format: **\+14325551212**\.

**preferred\_username**  
The `preferred_username` cannot be selected as both required and as an alias\. If the `preferred_username` is an alias, a user can add the attribute value once he or she is confirmed by using the [UpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html) API\.

 

### To edit standard attributes<a name="how-to-edit-standard-attributes"></a>

1. On the **Attributes** tab, choose the attributes you require for user registration\. If an attribute is required and a user doesn't provide the required attribute, the user cannot register\.
**Important**  
You cannot change these requirements after the user pool is created\.

1. To create an alias for email, phone number, address, or preferred username, choose **Alias**\. For more information on aliases, see [Overview of Aliases](#user-pool-settings-aliases)\.

1. Move on to create [Custom Attributes](#user-pool-settings-custom-attributes)\.

 

## Usernames and Preferred Usernames<a name="user-pool-settings-usernames"></a>

The `username` value is a separate attribute and not the same as the `name` attribute\. A `username` is always required to register a user, and it cannot be changed after a user is created\.

Developers can use the `preferred_username` attribute to give users a username that they can change\. For more information, see [Overview of Aliases](#user-pool-settings-aliases)\.

If your application does not require a username, you do not need to ask users to provide one\. Your app can create a unique username for users in the background\. This is useful if, for example, you want users to register and sign in with an email address and password\. For more information, see [Overview of Aliases](#user-pool-settings-aliases)\.

The `username` must be unique within a user pool\. A `username` can be reused, but only after it has been deleted and is no longer in use\.

## Overview of Aliases<a name="user-pool-settings-aliases"></a>

You can allow your end users to sign in with multiple identifiers by using aliases\.

By default, users sign in with their username and password\. The username is a fixed value that users cannot change\. If you mark an attribute as an alias, users can sign in using that attribute in place of the username\. The email address, phone number, and preferred username attributes can be marked as aliases\.

For example, if email and phone are selected as aliases for a user pool, users in that user pool can sign in using their username, email address, or phone number, along with their password\.

**Note**  
You can choose to sign in or sign up using either lowercase or uppercase letters in an alias when you configure your user pool for username case insensitivity\. For more information, see [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) in the *Amazon Cognito user pools API Reference*\.

If email is selected as an alias, a username cannot match a valid email format\. Similarly, if phone number is selected as an alias, a username that matches a valid phone number pattern is not accepted by the service for that user pool\.

**Note**  
Alias values must be unique in a user pool\. If an alias is configured for an email address or phone number, the value provided can be in a verified state in only one account\. During sign\-up, if an email address or phone number is supplied as an alias from a different account that has already been used, registration succeeds\. Nevertheless, when a user tries to confirm the account with this email \(or phone number\) and enters the valid code, an `AliasExistsException` error is thrown\. The error indicates to the user that an account with this email \(or phone number\) already exists\. At this point, the user can abandon the new account creation and can try to reset the password for the old account\. If the user continues creating the new account, your app should call the `ConfirmSignUp` API with the `forceAliasCreation` option\. This moves the alias from the previous account to the newly created account, and it also marks the attribute unverified in the previous account\.

Phone numbers and email addresses only become active aliases for a user after the phone numbers and email addresses are verified\. We therefore recommend that you choose automatic verification of email addresses and phone numbers if you use them as aliases\. The `preferred_username` attribute provides users the experience of changing their username, when in fact the actual username value for a user is not changeable\.

If you want to enable this user experience, submit the new `username` value as a `preferred_username` and choose `preferred_username` as an alias\. Then users can sign in with the new value they entered\.

If `preferred_username` is selected as an alias, the value can be provided only when an account is confirmed\. The value cannot be provided during registration\.

## Using Aliases to Simplify User Sign\-Up and Sign\-In<a name="user-pool-settings-aliases-settings"></a>

In the **Attributes** console tab, you can choose whether to allow your user to sign up with an email address or phone number as their username\.

**Note**  
This setting cannot be changed once the user pool is created\.

**Topics**
+ [Option 1: User Signs Up with Username and Signs In with Username or Alias](#user-pool-settings-aliases-settings-option-1)
+ [Option 2: User Signs Up and Signs In with Email or Phone Number Instead of Username](#user-pool-settings-aliases-settings-option-2)

### Option 1: User Signs Up with Username and Signs In with Username or Alias<a name="user-pool-settings-aliases-settings-option-1"></a>

In this case, the user signs up with a username\. In addition, you can optionally allow users to sign in with one or more of the following aliases:
+ a verified email address
+ a verified phone number
+ a preferred username

These aliases can be changed after the user signs up\.

Use the following steps to configure your user pool in the console to allow sign\-in with an alias\.

**To configure a user pool for sign\-in with an alias**

1. In the **Attributes** tab, under **How do you want your end users to sign\-up and sign\-in?**, select **Username**\.

1. Choose one of the following options:
   + **Also allow sign in with verified email address**: This allows users to sign in with their email address\.
   + **Also allow sign in with verified phone number**: This allows users to sign in with their phone number\.
   + **Also allow sign in with preferred username**: This allows users to sign in with a preferred username\. This is a username that the user can change\.

### Option 2: User Signs Up and Signs In with Email or Phone Number Instead of Username<a name="user-pool-settings-aliases-settings-option-2"></a>

In this case, the user signs up with an email address or phone number as their username\. You can choose whether to allow sign\-up with only email addresses, only phone numbers, or either one\.

The email or phone number must be unique, and it must not already be in use by another user\. It does not have to be verified\. After the user has signed up using an email or phone number, the user cannot create a new account with the same email or phone number; the user can only reuse the existing account \(and reset the password if needed\)\. However, the user can change the email or phone number to a new email or phone number; if it is not already in use, it becomes the new username\.

**Note**  
If users sign up with an email address as their username, they can change the username to another email address; they cannot change it to a phone number\. If they sign up with a phone number, they can change the username to another phone number; they cannot change it to an email address\.

Use the following steps to configure your user pool in the console to allow sign\-up and sign\-in with email or phone number\.

**To configure a user pool for sign\-up and sign\-in with email or phone number**

1. In the **Attributes** tab, under **How do you want your end users to sign\-up and sign\-in?**, select **Email address or phone number**\.

1. Choose one of the following options:
   + **Allow email addresses**: This allows your user to sign up with email as the username\.
   + **Allow phone numbers**: This allows your user to sign up with phone number as the username\.
   + **Allow both email addresses and phone number \(users can choose one\)**: This allows your user to use either an email address or a phone number as the username during sign\-up\.

**Note**  
You do not need to mark email or phone number as required attributes for your user pool\.

**To implement option 2 in your app**

1. Call the `CreateUserPool` API to create your user pool\. Set the `UserNameAttributes` parameter to `phone_number`, `email`, or `phone_number | email`\.

1. Call the `SignUp` API and pass an email address or phone number in the `username` parameter of the API\. This API does the following:
   + If the `username` string is in valid email format, the user pool automatically populates the `email` attribute of the user with the `username` value\.
   + If the `username` string is in valid phone number format, the user pool automatically populates the `phone_number` attribute of the user with the `username` value\.
   + If the `username` string format is not in email or phone number format, the `SignUp` API throws an exception\.
   + The `SignUp` API generates a persistent UUID for your user, and uses it as the immutable username attribute internally\. This UUID has the same value as the `sub` claim in the user identity token\.
   + If the `username` string contains an email address or phone number that is already in use, the `SignUp` API throws an exception\.

You can use an email address or phone number as an alias in place of the username in all APIs except the `ListUsers` API\. When you call `ListUsers`, you can search by the `email` or `phone_number` attribute; if you search by `username`, you must supply the actual username, not an alias\.

## Custom Attributes<a name="user-pool-settings-custom-attributes"></a>

You can add up to 25 custom attributes to your user pool\. You can specify a minimum and/or maximum length for custom attributes\. However, the maximum length for any custom attribute can be no more than 2048 characters\.

**Each custom attribute:**
+ Can be defined as a string or a number\.
+ Cannot be required\.
+ Cannot be removed or changed once added to the user pool\.
+ Can have a name with a character length that is within the limit that is accepted by Amazon Cognito\. For more information, see [Quotas in Amazon Cognito](limits.md)\.

**Note**  
In your code and in rules settings for [Role\-Based Access Control](role-based-access-control.md), custom attributes require the `custom:` prefix to distinguish them from standard attributes\.

**To add a custom attribute using the console**

1. From the navigation bar on the left choose **Attributes**\.

1. For each new attribute:

   1. Choose **Add another attribute** under **Do you want to add custom attributes?**\.

   1. Choose the properties for each custom attribute, such as the data **Type** \(string or number\), the **Name**, **Min length**, and **Max length**\.

   1. If you want to allow the user to change the value of a custom attribute after the value has been provided by the user, select **Mutable**\. 

## Attribute Permissions and Scopes<a name="user-pool-settings-attribute-permissions-and-scopes"></a>

You can set per\-app read and write permissions for each user attribute\. This gives you the ability to control which applications can see and/or modify each of the attributes that are stored for your users\. For example, you could have a custom attribute that indicates whether a user is a paying customer or not\. Your apps could see this attribute but could not modify it directly\. Instead, you would update this attribute using an administrative tool or a background process\. Permissions for user attributes can be set from the Amazon Cognito console, API, or CLI\. By default any new custom attributes will not be available until you set read and write permissions for them\.

**To set or change attribute permissions using the console**

1. From the navigation bar on the left choose **App clients**\.

1. Choose **Show Details** for the app client you want update\.

1. Choose **Set the attribute read and write permissions for each attribute\.**

1. Choose **Save app client changes**\.

Repeat these steps for each app client using the custom attribute\.

Attributes can be marked as readable or writable for each app\. This is true for both standard and custom attributes\. An app can read an attribute that is marked as readable and can write an attribute that is marked as writable\. If an app tries to update an attribute that is not writable, the app gets a `NotAuthorizedException` exception\. An app calling [GetUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html) only receives the attributes that are readable for that app\. The ID token issued post\-authentication only contains claims corresponding to the readable attributes\. Required attributes on a user pool are always writable\. If you, using CLI or the admin API, set a writable attribute and do not provide required attributes, then an `InvalidParameterException` exception is thrown\.

You can change attribute permissions and scopes after you have created your user pool\.