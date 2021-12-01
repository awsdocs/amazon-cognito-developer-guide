# Managing and searching for user accounts<a name="how-to-manage-user-accounts"></a>

Once you create your user pool, you can view and manage users using the AWS Management Console, as well as the AWS Command Line Interface or the Amazon Cognito API\. This topic describes how you can view and search for users using the AWS Management Console\.

## Viewing user attributes<a name="manage-user-accounts-viewing-user-attributes"></a>

Use the following procedure to view user attributes in the Amazon Cognito console\.

------
#### [ Original console ]

There are a number of operations you can perform in the AWS Management Console:
+ You can view the **Pool details** and edit user pool attributes, password policies, MFA settings, apps, and triggers\. For more information, see [User pools reference \(AWS Management Console\)](cognito-user-pools-getting-started-step-through-settings.md)\.
+ You can view the users in your user pool and drill down for more details\.
+ You can also view the details for an individual user in your user pool\.
+ You can also search for a user in your user pool\.

**To view user attributes**

1. From the Amazon Cognito home page in the AWS Management Console, choose **Manage user pools**\.

1. Choose your user pool from the **Your User Pools** page\.

1. Choose **User and Groups** to view user information\.

1. Choose a user name to show more information about an individual user\. From this screen, you can perform any of the following actions: 
   + **Add user to group**
   + **Reset user password**
   + **Confirm user**
   + **Enable or disable MFA**
   + **Delete user**

------
#### [ New console ]

**To view user attributes**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Users** tab, and then select a user in the list\.

1. On the user details page, under **User attributes**, you can view which attributes are associated with the user\.

------

## Resetting a user's password<a name="manage-user-accounts-reset-user-password"></a>

Use the following procedure to reset a user's password in the Amazon Cognito console\.

------
#### [ Original console ]

**To reset a user's password**

1. From the Amazon Cognito home page in the AWS Management Console, choose **Manage user pools**\.

1. Choose your user pool from the **Your User Pools** page\.

1. Choose **User and Groups** to view user information\.

1. Choose the user in the list for which you want to reset their password\.

   The **Reset user password** action immediately sends a confirmation code to the user, and disables the user’s current password by changing the user state to `RESET_REQUIRED`\. The **Enable MFA** action results in a confirmation code being sent to the user when the user tries to log in\. The **Reset user password** code is valid for 1 hour\. The MFA code is valid for 3 minutes\.

------
#### [ New console ]

**To reset a user's password**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Users** tab, and then select a user in the list\.

1. On the user details page, choose **Actions**, **Reset password**\.

1. In the **Reset password** dialog, review the information and when ready, choose **Reset**\.

   This action immediately results in a confirmation code being sent to the user and disables the user’s current password by changing the user state to `RESET_REQUIRED`\. The **Reset password** code is valid for 1 hour\.

------

## Searching user attributes<a name="manage-user-accounts-searching-user-attributes"></a>

If you have already created a user pool, you can search from the **Users** panel in the AWS Management Console\. You can also use the Amazon Cognito [ListUsers API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html), which accepts a **Filter** parameter\.

You can search for any of the following standard attributes\. Custom attributes are not searchable\.
+ username \(case\-sensitive\)
+ email
+ phone\_number
+ name
+ given\_name
+ family\_name
+ preferred\_username
+ cognito:user\_status \(called **Status** in the Console\) \(case\-insensitive\)
+ status \(called **Enabled** in the Console\) \(case\-sensitive\)
+ sub

**Note**  
You can also list users with a client\-side filter\. The server\-side filter matches no more than 1 attribute\. For advanced search, use a client\-side filter with the `--query` parameter of the `list-users` action in the AWS Command Line Interface\. When you use a client\-side filter, ListUsers returns a paginated list of zero or more users\. You can receive multiple pages in a row with zero results\. Repeat the query with each pagination token that is returned until you receive a null pagination token value, then review the combined result\.  
For more information about server\-side and client\-side filtering, see [Filtering AWS CLI output](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html) in the AWS Command Line Interface User Guide\.

## Searching for users with the AWS Management Console<a name="cognito-user-pools-manage-user-accounts-searching-for-users-using-console"></a>

If you have already created a user pool, you can search from the **Users** panel in the AWS Management Console\.

AWS Management Console searches are always prefix \("starts with"\) searches\.

------
#### [ Original console ]

All of the following examples use the same user pool\.

For example, if you want to list all users, leave the search box empty\.

![\[To list all users, leave the search box empty.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[To list all users, leave the search box empty.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[To list all users, leave the search box empty.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

If you want to search for all confirmed users, choose **Status** from the drop\-down menu\. In the search box, type the first letter of the word "confirmed\."

 

![\[To list all confirmed users, choose Status and type the first letter of the word "confirmed."\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[To list all confirmed users, choose Status and type the first letter of the word "confirmed."\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[To list all confirmed users, choose Status and type the first letter of the word "confirmed."\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

Note that some attribute values are case\-sensitive, such as **User name**\.

 

![\[If you type the wrong case for an attribute that is case-sensitive, the attribute value won't match.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[If you type the wrong case for an attribute that is case-sensitive, the attribute value won't match.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[If you type the wrong case for an attribute that is case-sensitive, the attribute value won't match.\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

------
#### [ New console ]

**To search for a user in the Amazon Cognito console**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Users** tab, and then enter in the user's username in the search field\. Note that some attribute values are case\-sensitive \(for example, **Username**\)\.

   You can also find users by adjusting the search filter to narrow the scope down to other user properties, such as **Email**, **Phone number**, or **Last name**\.

------

## Searching for users with the `ListUsers` API<a name="cognito-user-pools-searching-for-users-using-listusers-api"></a>

 To search for users from your app, use the Amazon Cognito [ListUsers API](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListUsers.html)\. This API uses the following parameters: 
+  `AttributesToGet`: An array of strings, where each string is the name of a user attribute to be returned for each user in the search results\. If the array is empty, all attributes are returned\.
+  `Filter`: A filter string of the form "`AttributeName` `Filter-Type` "`AttributeValue`""\. Quotation marks within the filter string must be escaped using the backslash \(`\`\) character\. For example, `"family_name = \"Reddy\""`\. If the filter string is empty, `ListUsers` returns all users in the user pool\. 
  +  `AttributeName`: The name of the attribute to search for\. You can only search for one attribute at a time\. 
**Note**  
You can only search for standard attributes\. Custom attributes are not searchable\. This is because only indexed attributes are searchable, and custom attributes cannot be indexed\.
  +  `Filter-Type`: For an exact match, use `=`, for example, `given_name = "Jon"`\. For a prefix \("starts with"\) match, use `^=`, for example, `given_name ^= "Jon"`\. 
  +  `AttributeValue`: The attribute value that must be matched for each user\.
+  `Limit`: Maximum number of users to be returned\.
+  `PaginationToken`: A token to get more results from a previous search\.
+  `UserPoolId`: The user pool ID for the user pool on which the search should be performed\.

All searches are case\-insensitive\. Search results are sorted by the attribute named by the `AttributeName` string, in ascending order\.

## Examples of using the `ListUsers` API<a name="cognito-user-pools-searching-for-users-listusers-api-examples"></a>

The following example returns all users and includes all attributes\.

```
{
    "AttributesToGet": [],
    "Filter": "",
    "Limit": 10,
    "UserPoolId": "us-east-1_samplepool"
}
```

The following example returns all users whose phone numbers start with "\+1312" and includes all attributes\.

```
{
    "AttributesToGet": [],
    "Filter": "phone_number ^= \"+1312\"",
    "Limit": 10,
    "UserPoolId": "us-east-1_samplepool"
}
```

The following example returns the first 10 users whose family name is "Reddy"\. For each user, the search results include the user's given name, phone number, and email address\. If there are more than 10 matching users in the user pool, the response includes a pagination token\.

```
{
    "AttributesToGet": [
        "given_name", "phone_number", "email"
    ],
    "Filter": "family_name = \"Reddy\"",
    "Limit": 10,
    "UserPoolId": "us-east-1_samplepool"
}
```

If the previous example returns a pagination token, the following example returns the next 10 users that match the same filter string\.

```
{
    "AttributesToGet": [
        "given_name", "phone_number", "email"
    ],
    "Filter": "family_name = \"Reddy\"",
    "Limit": 10,
    "PaginationToken": "pagination_token_from_previous_search",
    "UserPoolId": "us-east-1_samplepool"
}
```