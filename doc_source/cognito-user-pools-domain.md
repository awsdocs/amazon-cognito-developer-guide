# Adding a Domain Name for Your User Pool<a name="cognito-user-pools-domain"></a>

**Note**  
The **Domain name** tab appears only when you're editing an existing user pool\.

On the **Domain name** tab, you can enter your own prefix domain name\. The domain for your app is `https://<domain_prefix>.auth.<region>.amazoncognito.com`\.

The full URL for your app will look like this example: `https://example.auth.us-east-1.amazoncognito.com/login?redirect_uri=https://www.google.com&response_type=code&client_id=<client_id_value>`

For more information, see [Configuring a User Pool Domain](cognito-user-pools-assign-domain.md)\.

**Important**  
Before you can access the URL for your app, you must specify app client settings such as callback and redirect URLs\. For more information, see [Configuring App Client Settings](cognito-user-pools-app-settings.md)\.

**To specify a domain name for your user pool**

1. Enter the domain name you want in the **Prefix domain name** box\.

1. Choose **Check availability** as needed\.

1. Choose **Save changes**\.