# Message templates<a name="cognito-user-pool-settings-message-templates"></a>

You can use message templates to insert fields into your messages using placeholders that the corresponding values replace\.


**Template placeholders**  

|  Description  |  Token  | 
| --- | --- | 
| Verification code | \{\#\#\#\#\} | 
| Temporary password | \{\#\#\#\#\} | 
| User name | \{username\} | 

**Note**  
You can't use the `{username}` placeholder in verification email messages\. You can use the `{username}` placeholder in invitation email messages that you generate with the [AdminCreateUser](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminCreateUser.html) operation\. These invitation email messages use two placeholders: the user name, as `{username}`, and the temporary password, as `{####}`\. 

You can use advanced security template placeholders to do the following:
+ Include specific details about an event such as IP address, city, country, sign\-in time, and device name\. Amazon Cognito advanced security features can analyze these details\.
+ Verify whether a one\-click link is valid\.
+ Use event ID, feedback token, and user name to build your own one\-click link\.

**Note**  
To generate one\-clink links and use the `{one-click-link-valid}` and `{one-click-link-invalid}` placeholders in advanced security email templates, you must already have a domain configured for your user pool\.


**Advanced security template placeholders**  

|  Description  |  Token  | 
| --- | --- | 
| IP address | \{ip\-address\} | 
| City | \{city\} | 
| Country | \{country\} | 
| Log\-in time | \{login\-time\} | 
| Device name | \{device\-name\} | 
| One\-click link is valid | \{one\-click\-link\-valid\} | 
| One\-click link is not valid | \{one\-click\-link\-invalid\} | 
| Event ID | \{event\-id\} | 
| Feedback token | \{feedback\-token\} | 