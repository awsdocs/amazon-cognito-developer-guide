# Adding a domain name for your user pool<a name="cognito-user-pools-domain"></a>

------
#### [ Original console ]

**Note**  
The **Domain name** tab only appears when you're editing an existing user pool\.

On the **Domain name** tab, you can enter your own prefix domain name\. The domain for your app is `https://<domain_prefix>.auth.<region>.amazoncognito.com`\.

The full URL for your app will look like this example: 

`https://example.auth.us-east-1.amazoncognito.com/login?redirect_uri=https://www.google.com&response_type=code&client_id=<client_id_value>`

For more information, see [Configuring a user pool domain](cognito-user-pools-assign-domain.md)\.

**Important**  
Before you can access the URL for your app, you must specify app client settings such as callback and redirect URLs\. For more information, see [Configuring app client settings](cognito-user-pools-app-settings.md)\.

**To specify a domain name for your user pool**

1. Enter the prefix domain name you want in the **Prefix domain name** box\.

1. Choose **Check availability** to verify that your desired prefix domain name is available\.

1. If the prefix domain name is available for use, choose **Save changes**\.

------
#### [ New console ]

**Note**  
The **Domain name** tab only appears when you're editing an existing user pool\.

On the **App integration** tab, you create an Amazon\-owned prefix domain or use a custom domain for your user pool\. The prefix domain for your app is `https://<domain_prefix>.auth.<region>.amazoncognito.com`\.

The full URL for your app will look like this example: 

`https://example.auth.us-east-1.amazoncognito.com/login?redirect_uri=https://www.google.com&response_type=code&client_id=<client_id_value>`

For more information, see [Configuring a user pool domain](cognito-user-pools-assign-domain.md)\.

**Important**  
Before you can access the hosted UI for your app, you must specify app client settings such as callback and redirect URLs\. For more information, see [Configuring app client settings](cognito-user-pools-app-settings.md)\.

**Configure a domain**

1. Navigate to the **App integration** tab for your user pool\.

1. Next to **Domain**, choose **Actions** and select either **Create custom domain** or **Create Cognito domain**\. If you have already configured a user pool domain, choose **Delete Cognito domain** or **Delete custom domain** before creating your new custom domain\.

1. Enter an available domain prefix to use with a **Cognito domain**\. For information on setting up a **Custom domain**, see [Using your own Domain for the hosted UI](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-add-custom-domain.html)

1. Choose **Create**\.

------