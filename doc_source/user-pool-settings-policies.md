# Adding user pool password requirements<a name="user-pool-settings-policies"></a>

To create strong passwords for your app users, specify a minimum password length of at least eight characters\. Also require uppercase, numeric, and special characters\. Complex passwords are harder to guess\. We recommend them as a security best practice\.

You can use the following characters in passwords:
+ Uppercase and lowercase [Basic latin](http://memory.loc.gov/diglib/codetables/42.html) letters
+ Numbers
+ Special characters listed in the next section\.

## Creating a password policy<a name="user-pool-settings-password-policies"></a>

You can specify the following requirements in the **Password policy** in the AWS Management Console under the **Sign\-in experience** tab\.
+ A **Password minimum length** of at least six characters and at most 99 characters\. This is the shortest password you want your users to be able to set\. Amazon Cognito passwords can be up to 256 characters in length\.
+ Your users must create a password that **Contains at least 1** of the following types of characters\.
  + **Number**
  + **Special character** from the following set\. The space character is also treated as a special character\.

    `^ $ * . [ ] { } ( ) ? " ! @ # % & / \ , > < ' : ; | _ ~ ` = + -` 
  + **Uppercase [Basic Latin](http://memory.loc.gov/diglib/codetables/42.html) letter**
  + **Lowercase [Basic Latin](http://memory.loc.gov/diglib/codetables/42.html) letter**