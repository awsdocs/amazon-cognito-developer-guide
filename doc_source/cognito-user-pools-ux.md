# Customizing the Built\-in App UI to Sign Up and Sign In Users<a name="cognito-user-pools-ux"></a>

**Note**  
The **UI customization** tab appears only when you're editing an existing user pool\.

On the **UI customization** tab, you can add your own customizations to the default app UI\.

For detailed information about each of the customization fields, see [Customizing the Built\-in Sign\-in and Sign\-up Webpages](cognito-user-pools-app-ui-customization.md)\.

**Note**  
You can view the hosted UI with your customizations by constructing the following URL, with the specifics for your user pool, and typing it into a browser: ` https://<your_domain>/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>` You may have to wait up to one minute to refresh your browser before changes made in the console appear\.  
Your domain is shown on the **Domain name** tab\. Your app client ID and callback URL are shown on the **General settings** tab\.

**To customize the built\-in app UI**

1. Under **App client to customize**, choose the app you want to customize from the dropdown menu of app clients that you previously created in the **App clients** tab\.

1. To add a logo to the default app UI, choose **Choose a file** or drag a file onto the **Logo** box\.

1. Under **CSS customizations \(optional\)**, you can customize the appearance of the app by changing various properties from their default values\.

1. Choose **Save changes**\.