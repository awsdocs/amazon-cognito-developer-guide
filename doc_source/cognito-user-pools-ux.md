# Customizing the built\-in app UI to sign up and sign in users<a name="cognito-user-pools-ux"></a>

------
#### [ Original console ]

**Note**  
The **UI customization** tab only appears when you're editing an existing user pool\.

On the **UI customization** tab, you can add your own customizations to the default app UI\. For detailed information about each of the customization fields, see [Customizing the built\-in sign\-in and sign\-up webpages](cognito-user-pools-app-ui-customization.md)\.

**Note**  
You can view the hosted UI with your customizations by constructing the following URL, with the specifics for your user pool, and entering it into a browser: ` https://<your_domain>/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>` You may have to wait up to one minute to refresh your browser before changes made in the console appear\.  
Your domain is shown on the **Domain name** tab\. Your app client ID and callback URL are shown on the **General settings** tab\.

**To customize the built\-in app UI**

1. Under **App client to customize**, choose from the previously created app clients in the dropdown menu\.

1. To add a logo to the default app UI, choose **Choose a file**, or drag a file onto the **Logo** box\.

1. Under **CSS customizations \(optional\)**, you can customize the appearance of the app by changing the CSS properties for the UI from their default values\.

1. Choose **Save changes**\.

------
#### [ New console ]

With **Hosted UI customization** in the **App integration** tab, you can add your own customizations to the default app UI\. 

You can also customize the hosted UI for an app client\. Choose the app client from the **App integration** tab and locate **Hosted UI customization**\. Choose **Use client\-level settings** to enter the UI customization screen for that app client only\.

For detailed information about each of the customization fields, see [Customizing the built\-in sign\-in and sign\-up webpages](cognito-user-pools-app-ui-customization.md)\.

**Note**  
You can view the hosted UI with your customizations by constructing the following URL, with the specifics for your user pool, and typing it into a browser: ` https://<your_domain>/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>` You may have to wait up to one minute to refresh your browser before changes made in the console appear\.  
Your domain is shown on the **Domain name** tab\. Your app client ID and callback URL are shown on the **General settings** tab\.  
You can also launch the hosted UI for an app client that has it enabled\. Choose the app client in the **App integration** tab and locate **Hosted UI settings**\. Choose **View hosted UI**\.

**To customize the built\-in app UI**

1. To upload your own logo image file, choose **Choose file** or **Replace current file**\.

1. To customize the CSS for the hosted UI, download **CSS template\.css** and modify the template with the values you wish to customize\. Only the keys that are included in the template can be used with the hosted UI\. Added CSS keys will not be reflected in your UI\. After you have customized the CSS, choose **Choose file** or **Replace current file** to upload your custom CSS\.

1. Choose **Save changes**\.

------