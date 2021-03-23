# Default provider mappings<a name="provider-mappings"></a>

The following table has the default mapping information for the authentication providers that Amazon Cognito supports\.


| Provider | Token type | Principal tag values | Example | 
| --- | --- | --- | --- | 
|  Amazon Cognito user pool  |  ID token  |  aud\(client ID\) and sub\(user ID\)  |  "6jk8ltokc7ac9es6jrtg9q572f" , "57e7b692\-4f66\-480d\-98b8\-45a6729b4c88"   | 
|  Facebook  |  Access token  |  aud\(app\_id\), sub\(user\_id\)  |  "492844718097981", "112177216992379"  | 
|  Google  |  ID token  |  aud\(client ID\) and sub\(user ID\)  |  "620493171733\-eebk7c0hcp5lj3e1tlqp1gntt3k0rncv\.apps\.googleusercontent\.com", "109220063452404746097"  | 
|  SAML  |  Assertions  |  "http://schemas\.xmlsoap\.org/ws/2005/05/identity/claims/nameidentifier" , "http://schemas\.xmlsoap\.org/ws/2005/05/identity/claims/name"   |  "auth0\|5e28d196f8f55a0eaaa95de3" , "user123@gmail\.com"  | 
|  Apple  |  ID token  |  aud\(client ID\) and sub \(user ID\)  |  "com\.amazonaws\.ec2\-54\-80\-172\-243\.compute\-1\.client", "001968\.a6ca34e9c1e742458a26cf8005854be9\.0733"  | 
|  Amazon  |  Access token  |  aud \(Client ID on Amzn Dev Ac\), user\_id\(user ID\)  |  "amzn1\.application\-oa2\-client\.9d70d9382d3446108aaee3dd763a0fa6", "amzn1\.account\.AGHNIFJQMFSBG3G6XCPVB35ORQAA"  | 
|  Standard OIDC providers  |  ID and access tokens  |  aud \(as client\_id\), sub \(as user ID\)  |  "620493171733\-eebk7c0hcp5lj3e1tlqp1gntt3k0rncv\.apps\.googleusercontent\.com", "109220063452404746097"  | 
|  Twitter  |  Access token  |  aud \(app ID; app Secret\), sub \(user ID\)  |  "DfwifTtKEX1FiIBRnOTlR0CFK;Xgj5xb8xIrIVCPjXgLIdkW7fXmwcJJrFvnoK9gwZkLexo1y5z1" , "1269003884292222976"  | 
|  DevAuth  |  Map  |  Not applicable  |  "tag1", "tag2"  | 

**Note**  
The default attribute mappings option is automatically populated for the **Tag Key for Principal** and **Attribute** names\. You can't change default mappings\.