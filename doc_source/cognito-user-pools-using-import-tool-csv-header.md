# Creating the user import \.csv file<a name="cognito-user-pools-using-import-tool-csv-header"></a>

Before you can import your existing users into your user pool, you must create a `.csv` file that serves as the input\. To do this, download the user import `.csv` header information, and then you edit the file to match the formatting requirements outlined in [Formatting the \.csv file](#cognito-user-pools-using-import-tool-formatting-csv-file)\. 

## Downloading the \.csv file header \(console\)<a name="cognito-user-pools-using-import-tool-downloading-csv-header-console"></a>



1. Navigate to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), choose **Manage User Pools**, and then choose the user pool into which you are importing the users\.

1. Choose the **Users** tab\.

1. Choose **Import users**\.

1. Choose **Download CSV header** to get a `.csv` file containing the header row that you must include in your `.csv` file\.

## Downloading the \.csv file header \(AWS CLI\)<a name="cognito-user-pools-using-import-tool-downloading-csv-header-using-cli"></a>

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

## Formatting the \.csv file<a name="cognito-user-pools-using-import-tool-formatting-csv-file"></a>

 The downloaded user import `.csv` header file looks like this: 

```
cognito:username,name,given_name,family_name,middle_name,nickname,preferred_username,profile,picture,website,email,email_verified,gender,birthdate,zoneinfo,locale,phone_number,phone_number_verified,address,updated_at,cognito:mfa_enabled
```

You'll need to edit your `.csv` file so that it includes this header and the attribute values for your users, and is formatted according to the following rules:

**Note**  
For more information about attribute values, such as proper format for phone numbers, see [User pool attributes](user-pool-settings-attributes.md)\.
+ The first row in the file is the downloaded header row, which contains the user attribute names\.
+ The order of columns in the `.csv` file doesn't matter\.
+ Each row after the first row contains the attribute values for a user\.
+ All columns in the header must be present, but you don't need to provide values in every column\.
+ The following attributes are required:
  + **cognito:username**
  + **cognito:mfa\_enabled**
  + **email\_verified** or **phone\_number\_verified**
  + **email** \(if **email\_verified** is `true`\)
  + **phone\_number** \(if **phone\_number\_verified** is `true`\)
  + Any attributes that you marked as required when you created the user pool
+ The user pool must have at least one auto\-verified attribute, either **email\_verified** or **phone\_number\_verified**\. At least one of the auto\-verified attributes must be `true` for each user\. If the user pool has no auto\-verified attributes, the import job will not start\. If the user pool only has one auto\-verified attribute, that attribute must be verified for each user\. For example, if the user pool has only **phone\_number** as an auto\-verified attribute, the **phone\_number\_verified** value must be `true` for each user\.
**Note**  
In order for users to reset their passwords, they must have a verified email or phone number\. Amazon Cognito sends a message containing a reset password code to the email or phone number specified in the `.csv` file\. If the message is sent to the phone number, it is sent by SMS message\.
+ Attribute values that are strings should *not* be in quotation marks\.
+ If an attribute value contains a comma, you must put a backslash \(\\\) before the comma\. This is because the fields in a `.csv` file are separated by commas\.
+ The `.csv` file contents should be in UTF\-8 format without byte order mark\.
+ The **cognito:username** field is required and must be unique within your user pool\. It can be any Unicode string\. However, it cannot contain spaces or tabs\.
+ The **birthdate** values, if present, must be in the format **mm/dd/yyyy**\. This means, for example, that a birthdate of February 1, 1985 must be encoded as **02/01/1985**\.
+ The **cognito:mfa\_enabled** field is required\. If you've set multi\-factor authentication \(MFA\) to be required in your user pool, this field must be `true` for all users\. If you've set MFA to be off, this field must be `false` for all users\. If you've set MFA to be optional, this field can be either `true` or `false`, but it can't be empty\.
+ The maximum row length is 16,000 characters\.
+ The maximum `.csv` file size is 100 MB\.
+ The maximum number of rows \(users\) in the file is 500,000\. This maxiumum doesn't include the header row\.
+ The **updated\_at** field value is expected to be epoch time in seconds, for example: **1471453471**\.
+ Any leading or trailing white space in an attribute value will be trimmed\.

A complete sample user import `.csv` file looks like this:

```
cognito:username,name,given_name,family_name,middle_name,nickname,preferred_username,profile,picture,website,email,email_verified,gender,birthdate,zoneinfo,locale,phone_number,phone_number_verified,address,updated_at,cognito:mfa_enabled
John,,John,Doe,,,,,,,johndoe@example.com,TRUE,,02/01/1985,,,+12345550100,TRUE,123 Any Street,,FALSE
Jane,,Jane,Roe,,,,,,,janeroe@example.com,TRUE,,01/01/1985,,,+12345550199,TRUE,100 Main Street,,FALSE
```