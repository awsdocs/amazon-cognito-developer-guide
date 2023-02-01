# Customizing the built\-in sign\-in and sign\-up webpages<a name="cognito-user-pools-app-ui-customization"></a>

You can use the AWS Management Console, or the AWS CLI or API, to specify customization settings for the built\-in app UI experience\. You can upload a custom logo image to be displayed in the app\. You can also use cascading style sheets \(CSS\) to customize the look of the UI\.

You can specify app UI customization settings for a single client \(with a specific `clientId`\) or for all clients \(by setting the `clientId` to `ALL`\)\. If you specify `ALL`, the default configuration will be used for every client that has no UI customization set previously\. If you specify UI customization settings for a particular client, it will no longer fall back to the `ALL` configuration\.

The request that sets your UI customization must not exceed 135 KB in size\. In rare cases, the sum of request headers, your CSS file, and your logo might exceed 135KB\. Amazon Cognito encodes the image file to Base64\. This increases the size of a 100 KB image to 130 KB, leaving five KB for request headers and your CSS\. If the request is too large, the AWS Management Console or your `SetUICustomization` API request returns a `request parameters too large` error\. Adjust your logo image to be no greater than 100KB and your CSS file to be no larger than 3 KB\. You can't set CSS and logo customization separately\.

**Note**  
To customize your UI, you must set up a domain for your user pool\.

## Specifying a custom logo for the app<a name="cognito-user-pools-app-ui-customization-logo"></a>

Amazon Cognito centers your custom logo above the input fields at the [Login endpoint](login-endpoint.md)\.

Choose a PNG, JPG, or JPEG file that can scale to 350 by 178 pixels for your custom hosted UI logo\. Your logo file can be no larger than 100 KB in size, or 130 KB after Amazon Cognito encodes to to Base64\. To set an `ImageFile` in [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUICustomization.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUICustomization.html) in the API, you can convert your file to a Base64\-encoded text string or, in the AWS CLI, provide a file path and let Amazon Cognito encode it for you\.

## Specifying CSS customizations for the app<a name="cognito-user-pools-app-ui-customization-css"></a>

You can customize the CSS for the hosted app pages, with the following restrictions:
+ You can use any of the following CSS class names:
  + `background-customizable`
  + `banner-customizable`
  + `errorMessage-customizable`
  + `idpButton-customizable`
  + `idpButton-customizable:hover`
  + `inputField-customizable`
  + `inputField-customizable:focus`
  + `label-customizable`
  + `legalText-customizable`
  + `logo-customizable`
  + `submitButton-customizable`
  + `submitButton-customizable:hover`
  + `textDescription-customizable`
+ Property values can contain HTML, except for the following values: `@import`, `@supports`, `@page`, or `@media` statements, or Javascript\.

You can customize the following CSS properties\.

**Labels**  
+ **font\-weight** is a multiple of 100 from 100 to 900\.

**Input fields**  
+ **width** is the width of the containing block as a percentage\.
+ **height** is the height of the input field in pixels \(px\)\.
+ **color** is the text color\. It can be any standard CSS color value\.
+ **background\-color** is the background color of the input field\. It can be any standard CSS color value\.
+ **border** is a standard CSS border value that specifies the width, transparency, and color of the border of your app window\. Width can be any value from 1px to 100px\. Transparency can be solid or none\. Color can be any standard color value\.

**Text descriptions**  
+ **padding\-top** is the amount of padding above the text description\.
+ **padding\-bottom** is the amount of padding below the text description\.
+ **display** can be `block` or `inline`\.
+ **font\-size** is the font size for text descriptions\.

**Submit button**  
+ **font\-size** is the font size of the button text\.
+ **font\-weight** is the font weight of the button text: `bold`, `italic`, or `normal`\.
+ **margin** is a string of 4 values indicating the top, right, bottom, and left margin sizes for the button\.
+ **font\-size** is the font size for text descriptions\.
+ **width** is the width of the button text in percent of the containing block\.
+ **height** is the height of the button in pixels \(px\)\.
+ **color** is the button text color\. It can be any standard CSS color value\.
+ **background\-color** is the background color of the button\. It can be any standard color value\.

**Banner**  
+ **padding** is a string of 4 values indicating the top, right, bottom, and left padding sizes for the banner\.
+ **background\-color** is the banner's background color\. It can be any standard CSS color value\.

**Submit button hover**  
+ **color** is the foreground color of the button when you hover over it\. It can be any standard CSS color value\.
+ **background\-color** is the background color of the button when you hover over it\. It can be any standard CSS color value\.

**Identity provider button hover**  
+ **color** is the foreground color of the button when you hover over it\. It can be any standard CSS color value\.
+ **background\-color** is the background color of the button when you hover over it\. It can be any standard CSS color value\.

**Password check not valid**  
+ **color** is the text color of the `"Password check not valid"` message\. It can be any standard CSS color value\.

**Background**  
+ **background\-color** is the background color of the app window\. It can be any standard CSS color value\.

**Error messages**  
+ **margin** is a string of 4 values indicating the top, right, bottom, and left margin sizes\.
+ **padding** is the padding size\.
+ **font\-size** is the font size\.
+ **width** is the width of the error message as a percentage of the containing block\.
+ **background** is the background color of the error message\. It can be any standard CSS color value\.
+ **border** is a string of 3 values specifying the width, transparency, and color of the border\.
+ **color** is the error message text color\. It can be any standard CSS color value\.
+ **box\-sizing** is used to indicate to the browser what the sizing properties \(width and height\) should include\.

**Identity provider buttons**  
+ **height** is the height of the button in pixels \(px\)\.
+ **width** is the width of the button text as a percentage of the containing block\. 
+ **text\-align** is the text alignment setting\. It can be `left`, `right`, or `center`\.
+ **margin\-bottom** is the bottom margin setting\. 
+ **color** is the button text color\. It can be any standard CSS color value\.
+ **background\-color** is the background color of the button\. It can be any standard CSS color value\.
+ **border\-color** is the color of the button border\. It can be any standard CSS color value\.

**Identity provider descriptions**  
+ **padding\-top** is the amount of padding above the description\.
+ **padding\-bottom** is the amount of padding below the description\.
+ **display** can be `block` or `inline`\.
+ **font\-size** is the font size for descriptions\.

**Legal text**  
+ **color** is the text color\. It can be any standard CSS color value\.
+ **font\-size** is the font size\.
When you customize **Legal text**, you are customizing the message **We won't post to any of your accounts without asking first** that is displayed under social identity providers in the sign\-in page\.

**Logo**  
+ **max\-width** is the maximum width as a percentage of the containing block\.
+ **max\-height** is the maximum height as a percentage of the containing block\.

**Input field focus**  
+ **border\-color** is the color of the input field\. It can be any standard CSS color value\.
+ **outline** is the border width of the input field, in pixels\.

**Social button**  
+ **height** is the height of the button in pixels \(px\)\.
+ **text\-align** is the text alignment setting\. It can be `left`, `right`, or `center`\.
+ **width** is the width of the button text as a percentage of the containing block\.
+ **margin\-bottom** is the bottom margin setting\.

**Password check valid**  
+ **color** is the text color of the `"Password check valid"` message\. It can be any standard CSS color value\.

## Specifying app UI customization settings for a user pool \(AWS Management Console\)<a name="cognito-user-pools-app-ui-customization-console"></a>

You can use the AWS Management Console to specify UI customization settings for your app\.

------
#### [ Original console ]

**Note**  
You can view the hosted UI with your customizations by constructing the following URL, with the specifics for your user pool, and typing it into a browser: ` https://<your_domain>/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>` You may have to wait up to one minute to refresh your browser before changes made in the console appear\.  
Your domain is shown on the **Domain name** tab\. Your app client ID and callback URL are shown on the **General settings** tab\.

**To specify app UI customization settings**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. In the navigation pane, choose **Manage User Pools**, and choose the user pool you want to edit\.

1. Choose the **UI customization** tab\.

1. Under **App client to customize**, choose the app you want to customize from the dropdown menu of app clients that you previously created in the **App clients** tab\.

1. To upload your own logo image file, choose **Choose a file** or drag a file into the **Logo \(optional\)** box\.

1. Under **CSS customizations \(optional\)**, you can customize the appearance of the app by changing CSS properties from their default values\.

------
#### [ New console ]

**Note**  
You can view the hosted UI with your customizations by constructing the following URL, with the specifics for your user pool, and typing it into a browser: ` https://<your_domain>/login?response_type=code&client_id=<your_app_client_id>&redirect_uri=<your_callback_url>` You may have to wait up to one minute to refresh your browser before changes made in the console appear\.  
Your domain is shown on the **App integration** tab under **Domain**\. Your app client ID and callback URL are shown under **App clients**\.

**To specify app UI customization settings**

1. Sign in to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\.

1. In the navigation pane, choose **User Pools**, and choose the user pool you want to edit\.

1. Choose the **App integration** tab\.

1. To customize UI settings for all app clients, locate **Hosted UI customization** and select **Edit**\.

1. To customize UI settings for one app client, locate **App clients** and select the app client you wish to modify, then locate **Hosted UI customization** and select **Edit**\. To switch an app client from user pool default customization to client\-specific customization, select **Use client\-level settings**\.

1. To upload your own logo image file, choose **Choose file** or **Replace current file**\.

1. To customize hosted UI CSS, download **CSS template\.css** and modify the template with the values you wish to customize\. Only the keys that are included in the template can be used with the hosted UI\. Added CSS keys will not be reflected in your UI\. After you have customized the CSS file, choose **Choose file** or **Replace current file** to upload your custom CSS file\.

------

## Specifying app UI customization settings for a user pool \(AWS CLI and AWS API\)<a name="cognito-user-pools-app-ui-customization-cli-api"></a>

Use the following commands to specify app UI customization settings for your user pool\.

**To get the UI customization settings for a user pool's built\-in app UI, use the following API operations\.**
+ AWS CLI: `aws cognito-idp get-ui-customization`
+ AWS API: [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUICustomization.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetUICustomization.html)

**To set the UI customization settings for a user pool's built\-in app UI, use the following API operations\.**
+ AWS CLI from image file: `aws cognito-idp set-ui-customization --user-pool-id <your-user-pool-id> --client-id <your-app-client-id> --image-file fileb://"<path-to-logo-image-file>" --css ".label-customizable{ color: <color>;}"`
+ AWS CLI with image encoded as Base64 binary text: `aws cognito-idp set-ui-customization --user-pool-id <your-user-pool-id> --client-id <your-app-client-id> --image-file <base64-encoded-image-file> --css ".label-customizable{ color: <color>;}"`
+ AWS API: [https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUICustomization.html](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SetUICustomization.html)