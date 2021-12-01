# Adding user pool password requirements<a name="user-pool-settings-policies"></a>

Specifying a minimum password length of at least 8 characters, as well as requiring uppercase, numeric, and special characters, creates strong passwords for your app users\. Complex passwords are harder to guess, and we recommend them as a security best practice\.

These characters are allowed in passwords:
+ Uppercase and lowercase [Basic latin](http://memory.loc.gov/diglib/codetables/42.html) letters
+ Numbers
+ Special characters listed in the next section\.

## Creating a password policy<a name="user-pool-settings-password-policies"></a>

You can specify the following password requirements in the AWS Management Console:
+ **Minimum length**, which must be at least 6 characters but fewer than 99 characters
+ **Require numbers**
+ **Require a special character** from this set:

  `^ $ * . [ ] { } ( ) ? " ! @ # % & / \ , > < ' : ; | _ ~ `` 
+ **Require uppercase [Basic Latin](http://memory.loc.gov/diglib/codetables/42.html) letters**
+ **Require lowercase [Basic Latin](http://memory.loc.gov/diglib/codetables/42.html) letters**