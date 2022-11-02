# User pool attributes<a name="user-pool-settings-attributes"></a>

Attributes are pieces of information that help you identify individual users, such as name, email address, and phone number\. A new user pool has a set of default *standard attributes*\. You can also add custom attributes to your user pool definition in the AWS Management Console\. This topic describes those attributes in detail and gives you tips on how to set up your user pool\.

Don't store all information about your users in attributes\. For example, keep user data that changes frequently, such as usage statistics or game scores, in a separate data store, such as Amazon Cognito Sync or Amazon DynamoDB\.

## Standard attributes<a name="cognito-user-pools-standard-attributes"></a>

Amazon Cognito assigns all users the following set of standard attributes based on the [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#StandardClaims)\. 
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

These attributes are available as optional attributes for all users\. To make an attribute required, during the user pool creation process, select the **Required** check box next to the attribute\.

**Note**  
 When you mark a standard attribute as **Required**, a user can't register unless they provide a value for the attribute\. To create users and not give values for required attributes, administrators can use the [AdminCreateUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminCreateUser.html) API\. After you create a user pool, you can't switch an attribute between required and not required\.

By default, standard and custom attribute values can be any string with a length of up to 2048 characters, but some attribute values, such as `updated_at`, have format restrictions\. Only **email** and **phone** can be verified\.

**Note**  
Some documentation and standards refer to attributes as *members*\.

Here are some additional notes about some of the these fields\.

**email**  
Users and administrators can verify email address values\.  
An administrator with proper AWS account permissions can change the user's email address and also mark it as verified\. Mark an email address as verified with the [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) API or the [admin\-update\-user\-attributes](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-user-attributes.html) AWS Command Line Interface \(AWS CLI\) command\. With this command, the administrator can change the `email_verified` attribute to `true`\. You can also edit a user in the **Users** tab of the AWS Management Console to mark an email address as verified\.

**phone**  
A user must provide a phone number if SMS multi\-factor authentication \(MFA\) is active\. For more information, see [Adding MFA to a user pool](user-pool-settings-mfa.md)\.  
Users and administrators can verify phone number values\.  
An administrator with proper AWS account permissions can change the user's phone number and also mark it as verified\. Mark a phone number as verified with the [AdminUpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminUpdateUserAttributes.html) API or the [admin\-update\-user\-attributes](https://docs.aws.amazon.com/cli/latest/reference/cognito-idp/admin-update-user-attributes.html) AWS CLI command\. With this command, the administrator can change the `phone_number_verified` attribute to `true`\. You can also edit a user in the **Users** tab of the AWS Management Console to mark a phone number as verified\.  
Phone numbers must follow these format rules: A phone number must start with a plus \(**\+**\) sign, followed immediately by the country code\. A phone number can only contain the **\+** sign and digits\. Remove any other characters from a phone number, such as parentheses, spaces, or dashes \(**\-**\) before you submit the value to the service\. For example, a phone number based in the United States must follow this format: **\+14325551212**\.

**preferred\_username**  
You can select `preferred_username` as required or as an alias, but not both\. If the `preferred_username` is an alias, you can the [UpdateUserAttributes](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateUserAttributes.html) API to add the attribute value after you confirm the user\.

### View required attributes<a name="how-to-edit-standard-attributes"></a>

Use the following procedure to view required attributes for a given user pool\.

**Note**  
You can't change required attributes after you create a user pool\.

------
#### [ Original console ]

**To view required attributes**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose a user pool\.

1. In the navigation menu on the left, choose **Attributes**\.

1. Under **Which standard attributes are required?**, view the required attributes of your user pool\.

------
#### [ New console ]

**To view required attributes**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Sign\-up experience** tab\.

1. In the **Required attributes** section, view the required attributes of your user pool\.

------

## User name and preferred user name<a name="user-pool-settings-usernames"></a>

The `username` value is a separate attribute and not the same as the `name` attribute\. Each user has a `username` attribute\. Amazon Cognito automatically generates a user name for federated users\. You must provide a `username` attribute to create a native user in the Amazon Cognito directory\. After you create a user, you can't change the value of the `username` attribute\.

Developers can use the `preferred_username` attribute to give users user names that they can change\. For more information, see [Aliases](#user-pool-settings-aliases)\.

If your application doesn't require a user name, you don't need to ask users to provide one\. Your app can create a unique username for users in the background\. This can be useful if you want users to register and sign in with an email address and password\. For more information, see [Aliases](#user-pool-settings-aliases)\.

The `username` must be unique within a user pool\. A `username` can be reused, but only after you delete it and it is no longer in use\.

## Aliases<a name="user-pool-settings-aliases"></a>

If you want, your users can use aliases to enter other attributes when they sign in\. By default, users sign in with their user name and password\. The user name is a fixed value that users can't change\. If you mark an attribute as an alias, users can sign in with that attribute in place of the user name\. You can mark the email address, phone number, and preferred username attributes as aliases\. For example, if you select email address and phone number as aliases for a user pool, users in that user pool can sign in with their user name, email address, or phone number, along with their password\.

**Note**  
When you configure your user pool to be case insensitive, a user can use either lowercase or uppercase letters to sign up or sign in with their alias\. For more information, see [CreateUserPool](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_CreateUserPool.html) in the *Amazon Cognito user pools API Reference*\.

If you select email address as an alias, Amazon Cognito doesn't accept a user name that matches a valid email address format\. Similarly, if you select phone number as an alias, Amazon Cognito doesn't accept a user name for that user pool that matches a valid phone number format\.

**Note**  
Alias values must be unique in a user pool\. If you configure an alias for an email address or phone number, the value that you provide can be in a verified state in only one account\. During sign\-up, if your user provides an email address or phone number as an alias value and another user has already used that alias value, registration succeeds\. However, when a user tries to confirm the account with this email \(or phone number\) and enters the valid code, Amazon Cognito returns an `AliasExistsException` error\. The error indicates to the user that an account with this email address \(or phone number\) already exists\. At this point, the user can abandon their attempt to create the new account and instead try to reset the password for the old account\. If the user continues to create the new account, your app must call the `ConfirmSignUp` API with the `forceAliasCreation` option\. `ConfirmSignUp` with `forceAliasCreation` moves the alias from the previous account to the newly created account, and marks the attribute unverified in the previous account\.

Phone numbers and email addresses only become active aliases for a user after your user verifies the phone numbers and email addresses\. Therefore, we recommend that you choose automatic verification of email addresses and phone numbers if you use them as aliases\. 

Activate the `preferred_username` attribute so that your user can change the user name that they use to sign in while their `username` attribute value doesn't change\. If you want to set up this user experience, submit the new `username` value as a `preferred_username` and choose `preferred_username` as an alias\. Then users can sign in with the new value that they entered\. If you select `preferred_username` as an alias, your user can provide the value only when they confirm an account\. They can't provide the value during registration\.

### Using aliases to simplify user sign\-up and sign\-in<a name="user-pool-settings-aliases-settings"></a>

When you create a user pool, you can choose if you want your user to sign up with an email address or phone number as their user name\.

**Note**  
After you create a user pool, you can't change this setting\.

#### Option 1: User signs up with user name and signs in with user name or alias<a name="user-pool-settings-aliases-settings-option-1"></a>

When the user signs up with a user name, you can choose if they can sign in with one or more of the following aliases:
+ Verified email address
+ Verified phone number
+ Preferred user name

After the user signs up, they can change these aliases\.

Include the following steps when you create the user pool so that users can sign in with an alias\.

------
#### [ Original console ]

**To configure a user pool for sign\-in with an alias**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool**\.

1. In the top\-left corner of the page, choose **Attributes**\.

1. In the **Attributes** tab, under **How do you want your end users to sign\-in?**, select **Username**\. Then choose one of the following options:
   + **Also allow sign in with verified email address**: Allows users to sign in with their email address\.
   + **Also allow sign in with verified phone number**: Allows users to sign in with their phone number\.
   + **Also allow sign in with preferred username**: Allows users to sign in with a preferred user name\. This is a user name that the user can change\.

1. Choose **Next step** to save, and then complete all the steps in the wizard\.

------
#### [ New console ]

**To configure a user pool so that users can sign in with a preferred user name**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool** to start the user pool creation wizard\.

1. In **Configure sign\-in experience**, choose the identity **Provider types** that you want to associate with your user pool\.

1. Under **Cognito user pool sign\-in options**, choose any combination of **User name**, **Email**, and **Phone number**\.

1. Under **User name requirements**, choose **Allow users to sign in with a preferred user name** so that your users can set an alternate user name when they sign in\.

1. Choose **Next**, and then complete all of the steps in the wizard\.

------

#### Option 2: User signs up and signs in with email address or phone number instead of user name<a name="user-pool-settings-aliases-settings-option-2"></a>

When the user signs up with an email address or phone number as their user name, you can choose if they can sign up with only email addresses, only phone numbers, or either one\.

The email address or phone number must be unique, and it must not already be in use by another user\. It doesn't have to be verified\. After the user has signed up with an email address or phone number, the user can't create a new account with the same email address or phone number\. The user can only reuse the existing account and reset the account password, if needed\. However, the user can change the email address or phone number to a new email address or phone number\. If the email address or phone number isn't already in use, it becomes the new user name\.

**Note**  
If a user signs up with an email address as their username, they can change the user name to another email address, but they can't change it to a phone number\. If they sign up with a phone number, they can change the user name to another phone number, but they can't change it to an email address\.

Use the following steps during the user pool creation process to set up sign\-up and sign\-in with email address or phone number\.

------
#### [ Original console ]

**To configure a user pool for sign\-up and sign\-in with email address or phone number**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool**\.

1. In the top\-left corner of the page, choose **Attributes**\.

1. In the **Attributes** tab, under **How do you want your end users to sign\-in?**, select **Email address or phone number**\. Then choose one of the following options:
   + **Allow email addresses**: Allows your user to sign up with email as the user name\.
   + **Allow phone numbers**: Allows your user to sign up with phone number as the user name\.
   + **Allow both email addresses and phone number \(users can choose one\)**: Allows your user to use either an email address or a phone number as the user name when the user signs up\.

1. Choose **Next step** to save, and then complete all the steps in the wizard\.

------
#### [ New console ]

**To configure a user pool for sign\-up and sign\-in with email address or phone number**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **User Pools**\.

1. In the top\-right corner of the page, choose **Create a user pool** to start the user pool creation wizard\.

1. Under **Cognito user pool sign\-in options**, choose any combination of **User name**, **Email**, and **Phone number** that represents the alias attributes that the user can use to sign in\.

1. Under **User name requirements**, choose one or both of the following options:  
****Allow users to sign in with a preferred user name****  
Allows the user to sign in with a preferred user name\. The user can change this user name\.  
****Make user name case sensitive****  
Sets the user name as case sensitive, where **ExampleUser** and **exampleuser** are unique user names\.

1. Choose **Next**, and then complete all the steps in the wizard\.

------

**Note**  
You do not need to mark email address or phone number as required attributes for your user pool\.

**To implement option 2 in your app**

1. Call the `CreateUserPool` API to create your user pool\. Set the `UserNameAttributes` parameter to `phone_number`, `email`, or `phone_number | email`\.

1. Call the `SignUp` API and pass an email address or phone number in the `username` parameter of the API\. This API does the following:
   + If the `username` string is in valid email address format, the user pool automatically populates the `email` attribute of the user with the `username` value\.
   + If the `username` string is in valid phone number format, the user pool automatically populates the `phone_number` attribute of the user with the `username` value\.
   + If the `username` string format isn't in email address or phone number format, the `SignUp` API returns an exception\.
   + The `SignUp` API generates a persistent UUID for your user, and uses it internally as the immutable user name attribute\. This UUID has the same value as the `sub` claim in the user identity token\.
   + If the `username` string contains an email address or phone number that is already in use, the `SignUp` API returns an exception\.

You can use an email address or phone number as an alias in place of the user name in all APIs except the `ListUsers` API\. When you call `ListUsers`, you can search by the `email` or `phone_number` attribute\. If you search by `username`, you must supply the actual user name, not an alias\.

## Custom attributes<a name="user-pool-settings-custom-attributes"></a>

You can add up to 50 custom attributes to your user pool\. You can specify a minimum and/or maximum length for custom attributes\. However, the maximum length for any custom attribute can be no more than 2048 characters\.

**Each custom attribute has the following characteristics:**
+ You can define it as a string or a number\. Amazon Cognito writes custom attribute values to the ID token only as strings\.
+ You can't require that users provide a value for the attribute\.
+ You can't remove or change it after you add it to the user pool\.
+ The character length of the attribute name is within the limit that Amazon Cognito accepts\. For more information, see [Quotas in Amazon Cognito](limits.md)\.
+ It can be *mutable* or *immutable*\. You can only write a value to an immutable attribute when you create a user\. You can change the value of a mutable attribute if your app client has write permission to the attribute\. See [Attribute permissions and scopes](#user-pool-settings-attribute-permissions-and-scopes) for more information\.

**Note**  
In your code, and in rules settings for [Role\-based access control](role-based-access-control.md), custom attributes require the `custom:` prefix to distinguish them from standard attributes\.

Use the following procedure to create a new custom attribute\.

------
#### [ Original console ]

**To add a custom attribute through the console**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to configure\.

1. From the left navigation bar, choose **Attributes**\.

1. Under **Do you want to add custom attributes?**, choose **Add custom attribute**\.

1. Provide the following details about the new attribute:
   + Enter a **Name**\.
   + Select a** Type** of either **String** or **Number**\.
   + Enter a **Min** string length or number value\.
   + Enter a **Max** string length or number value\.
   + Select **Mutable** if you want to give users permission to change the value of a custom attribute after they set the initial value\.

1. Choose **Save changes**\.

------
#### [ New console ]

**To add a custom attribute using the console**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Sign\-up experience** tab, and in the **Custom attributes** section, choose **Add custom attributes**\.

1. On the **Add custom attributes** page, provide the following details about the new attribute:
   + Enter a **Name**\.
   + Select a** Type** of either **String** or **Number**\.
   + Enter a **Min** string length or number value\.
   + Enter a **Max** string length or number value\.
   + Select **Mutable** if you want to give users permission to change the value of a custom attribute after they set the initial value\.

1. Choose **Save changes**\.

------

## Attribute permissions and scopes<a name="user-pool-settings-attribute-permissions-and-scopes"></a>

For each app client, you can set read and write permissions for each user attribute\. This way, you can control the access that any app has to read and modify each attribute that you store for your users\. For example, you might have a custom attribute that indicates whether a user is a paying customer or not\. Your apps might be able to see this attribute but not change it directly\. Instead, you would update this attribute using an administrative tool or a background process\. You can set permissions for user attributes from the Amazon Cognito console, the Amazon Cognito API, or the AWS CLI\. By default, any new custom attributes aren't available until you set read and write permissions for them\. When you create a new app client in the Amazon Cognito console, you grant your app read and write permissions for all standard and custom attributes by default\. To limit your app to only the amount of information that it requires, choose a subset of scopes that your app needs\.

------
#### [ Original console ]

**To update attribute permissions through the console**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. On the **Your User Pools** page, choose the user pool that you want to configure\.

1. In the left navigation bar, choose **App clients**\.

1. Choose **Show Details** for the app client that you want to update\.

1. At the bottom of the page, choose **Set attribute read and write permissions**, and then configure your read and write permissions\.

1. Choose **Save app client changes**\.

------
#### [ New console ]

**To update attribute permissions through the console**

1. Go to [Amazon Cognito](https://console.aws.amazon.com/cognito/home) in the AWS Management Console\. If the console prompts you, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **App integration** tab, and in the **App clients** section, choose an app client from the list\.

1. In the **Attribute read and write permissions** section, choose **Edit**\.

1. On the **Edit attribute read and write permissions** page, configure your read and write permissions, and then choose **Save changes**\.

------

Repeat these steps for each app client that uses the custom attribute\.

For each app\. you can mark attributes as readable or writable\. This applies to both standard and custom attributes\. An app can read an attribute that you mark as readable and can write an attribute that you mark as writable\. If an app tries to update an attribute that isn't writable, the app gets a `NotAuthorizedException` exception\. An app that calls [GetUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUser.html) only receives the attributes that are readable for that app\. Your user's ID token from an app only contains claims that correspond to the readable attributes\. Required attributes in a user pool are always writable\. If you use the AWS CLI or the admin API to set a writable attribute and don't provide required attributes, then you receive an `InvalidParameterException` exception\.

You can change attribute permissions and scopes after you have created your user pool\.