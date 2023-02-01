# Creating the user import CSV file<a name="cognito-user-pools-using-import-tool-csv-header"></a>

Before you can import your existing users into your user pool, you must create a comma\-separated values \(CSV\) file that contains the users that you want to import, and their attributes\. From your user pool, you can retrieve a user import file with headers that reflect the attribute schema of your user pool\. You can then insert user information that matches the formatting requirements in [Formatting the CSV file](#cognito-user-pools-using-import-tool-formatting-csv-file)\. 

## Downloading the CSV file header \(console\)<a name="cognito-user-pools-using-import-tool-downloading-csv-header-console"></a>



Use the following procedure to download the CSV header file\.

------
#### [ Original console ]

**To download the CSV file header**

1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), choose **Manage User Pools**, and then choose the user pool that you are importing the users into\.

1. Choose the **Users** tab\.

1. Choose **Import users**\.

1. Choose **Download CSV header** to get a CSV file containing the header row that you must include in your CSV file\.

------
#### [ New console ]

**To download the CSV file header**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. You might be prompted for your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list\.

1. Choose the **Users** tab\.

1. In the **Import users** section, choose **Create an import job**\.

1. Under **Upload CSV**, select the *template\.csv* link and download the CSV file\.

------

## Downloading the CSV file header \(AWS CLI\)<a name="cognito-user-pools-using-import-tool-downloading-csv-header-using-cli"></a>

To get a list of the correct headers, run the following CLI command, where *USER\_POOL\_ID* is the user pool identifier for the user pool you'll import users into:

```
aws cognito-idp get-csv-header --user-pool-id "USER_POOL_ID"
```

Sample response:

```
{
    "CSVHeader": [
        "name",
        "given_name",
        "family_name",
        "middle_name",
        "nickname",
        "preferred_username",
        "profile",
        "picture",
        "website",
        "email",
        "email_verified",
        "gender",
        "birthdate",
        "zoneinfo",
        "locale",
        "phone_number",
        "phone_number_verified",
        "address",
        "updated_at",
        "cognito:mfa_enabled",
        "cognito:username"
    ],
    "UserPoolId": "USER_POOL_ID"
}
```

## Formatting the CSV file<a name="cognito-user-pools-using-import-tool-formatting-csv-file"></a>

 The downloaded user import CSV header file looks like the following string\. It also includes any custom attributes you have added to your user pool\.

```
cognito:username,name,given_name,family_name,middle_name,nickname,preferred_username,profile,picture,website,email,email_verified,gender,birthdate,zoneinfo,locale,phone_number,phone_number_verified,address,updated_at,cognito:mfa_enabled
```

Edit your CSV file so that it includes this header and the attribute values for your users, and is formatted according to the following rules:

**Note**  
For more information about attribute values, such as proper format for phone numbers, see [User pool attributes](user-pool-settings-attributes.md)\.
+ The first row in the file is the downloaded header row, which contains the user attribute names\.
+ The order of columns in the CSV file doesn't matter\.
+ Each row after the first row contains the attribute values for a user\.
+ All columns in the header must be present, but you don't need to provide values in every column\.
+ The following attributes are required:
  + **cognito:username**
  + **cognito:mfa\_enabled**
  + **email\_verified** or **phone\_number\_verified**
    + At least one of the auto\-verified attributes must be `true` for each user\. An auto\-verified attribute is an email address or phone number that Amazon Cognito automatically sends a code to when a new user joins your user pool\.
    + The user pool must have at least one auto\-verified attribute, either **email\_verified** or **phone\_number\_verified**\. If the user pool has no auto\-verified attributes, the import job will not start\.
    + If the user pool only has one auto\-verified attribute, that attribute must be verified for each user\. For example, if the user pool has only **phone\_number** as an auto\-verified attribute, the **phone\_number\_verified** value must be `true` for each user\.
**Note**  
For users to reset their passwords, they must have a verified email or phone number\. Amazon Cognito sends a message containing a reset password code to the email or phone number specified in the CSV file\. If the message is sent to the phone number, it is sent by SMS message\. For more information, see [Verifying contact information at sign\-up](signing-up-users-in-your-app.md#allowing-users-to-sign-up-and-confirm-themselves)\.
  + **email** \(if **email\_verified** is `true`\)
  + **phone\_number** \(if **phone\_number\_verified** is `true`\)
  + Any attributes that you marked as required when you created the user pool
+ Attribute values that are strings should *not* be in quotation marks\.
+ If an attribute value contains a comma, you must put a backslash \(\\\) before the comma\. This is because the fields in a CSV file are separated by commas\.
+ The CSV file contents should be in UTF\-8 format without byte order mark\.
+ The **cognito:username** field is required and must be unique within your user pool\. It can be any Unicode string\. However, it cannot contain spaces or tabs\.
+ The **birthdate** values, if present, must be in the format **mm/dd/yyyy**\. This means, for example, that a birthdate of February 1, 1985 must be encoded as **02/01/1985**\.
+ The **cognito:mfa\_enabled** field is required\. If you've set multi\-factor authentication \(MFA\) to be required in your user pool, this field must be `true` for all users\. If you've set MFA to be off, this field must be `false` for all users\. If you've set MFA to be optional, this field can be either `true` or `false`, but it can't be empty\.
+ The maximum row length is 16,000 characters\.
+ The maximum CSV file size is 100 MB\.
+ The maximum number of rows \(users\) in the file is 500,000\. This maximum doesn't include the header row\.
+ The **updated\_at** field value is expected to be epoch time in seconds, for example: **1471453471**\.
+ Any leading or trailing white space in an attribute value will be trimmed\.

The following list is a example CSV import file for a user pool with no custom attributes\. Your user pool schema might differ from this example\. In that case, you must provide test values in the CSV template that you download from your user pool\.

```
cognito:username,name,given_name,family_name,middle_name,nickname,preferred_username,profile,picture,website,email,email_verified,gender,birthdate,zoneinfo,locale,phone_number,phone_number_verified,address,updated_at,cognito:mfa_enabled
John,,John,Doe,,,,,,,johndoe@example.com,TRUE,,02/01/1985,,,+12345550100,TRUE,123 Any Street,,FALSE
Jane,,Jane,Roe,,,,,,,janeroe@example.com,TRUE,,01/01/1985,,,+12345550199,TRUE,100 Main Street,,FALSE
```