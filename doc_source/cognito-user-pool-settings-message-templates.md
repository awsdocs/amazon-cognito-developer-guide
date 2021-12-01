# Message templates<a name="cognito-user-pool-settings-message-templates"></a>

Message templates allow you to insert a field into your message using a placeholder that will be replaced with the corresponding value\.


**Template placeholders**  

|  Description  |  Token  | 
| --- | --- | 
| Verification code | \{\#\#\#\#\} | 
| Temporary password | \{\#\#\#\#\} | 
| User name | \{username\} | 

**Note**  
\{username\} can't be used in verification emails\. \{username\} is available for invitation emails sent after AdminCreateUser call\. These invitation emails provide two place holders: username \{username\} and temporary password \{\#\#\#\#\} 

You can use advanced security template placeholders to:
+ Include specific details about an event such as IP address, city, country, login time, and device name, for analysis by Amazon Cognito advanced security features\.
+ Verify whether a one\-click link is valid\.
+ Build your own one\-click link using event ID, feedback token, and username\.


**Advanced security template placeholders**  

|  Description  |  Token  | 
| --- | --- | 
| IP address | \{ip\-address\} | 
| City | \{city\} | 
| Country | \{country\} | 
| Login time | \{login\-time\} | 
| Device name | \{device\-name\} | 
| One click link is valid | \{one\-click\-link\-valid\} | 
| One click link is invalid | \{one\-click\-link\-invalid\} | 
| Event ID | \{event\-id\} | 
| Feedback token | \{feedback\-token\} | 