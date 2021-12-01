# Specifying user pool device tracking settings<a name="amazon-cognito-user-pools-device-tracking"></a>

As a way of providing additional security, you can track devices that users have logged in to\. This topic describes how to add device tracking to your Amazon Cognito user pools in the AWS Management Console\.

## Setting up remembered devices<a name="amazon-cognito-user-pools-setting-up-remembered-devices"></a>

With Amazon Cognito user pools, you can choose to have Amazon Cognito remember devices used to access your application and associate these remembered devices with your application's users in a user pool\. You can also choose to use remembered devices to stop sending codes to your users when you have set up multi\-factor authentication \(MFA\)\. You must use the USER\_SRP\_AUTH authentication flow to use the device tracking feature\. You must also enable MFA for your user pool\.

When setting up the remembered devices functionality through the Amazon Cognito console, you have three options: **Always**, **User Opt\-In**, and **No**\.

**Note**  
If you have opted in to the new Amazon Cognito console, your device tracking options are **Always**, **User Opt\-In**, and **Don't remember**\.
+ **No** \(default\) – Devices are not remembered\.
+ **Always** – Every device used by your application's users is remembered\.
+ **User Opt\-In** – Your user's device is only remembered if that user opts to remember the device\.

If either **Always** or **User Opt\-In** is selected, a device identifier \(key and secret\) will be assigned to each device the first time a user signs in with that device\. This key will not be used for anything other than identifying the device, but it will be tracked by the service\. 

If you select **Always**, Amazon Cognito will use the device identifier \(key and secret\) to authenticate the device on every user sign\-in with that device as part of the user authentication flow\. 

If you select **User Opt\-In**, you can remember devices only when your application's users opt to do so\. When a user signs in with a new device, the response from the request to initiate tracking indicates whether the user should be prompted about remembering their device\. You must create the user interface to prompt users\. If the user opts to have the device remembered, the device status is updated with a 'remembered' state\. 

The AWS Mobile SDKs have additional APIs to see remembered devices \([ListDevices](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ListDevices.html), [GetDevice](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_GetDevice.html)\), mark a device as remembered or not remembered \([UpdateDeviceStatus](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_UpdateDeviceStatus.html)\), and stop tracking a device \([ForgetDevice](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_ForgetDevice.html)\)\. In the REST API, there are additional administrator versions of these APIs that have elevated privileges and work on any user\. They have API names such as [AdminListDevices](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminListDevices.html), [AdminGetDevice](https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_AdminGetDevice.html), and so on\. They are not exposed through the SDKs\. 

## Using remembered devices to suppress multi factor authentication \(MFA\)<a name="amazon-cognito-user-pools-using-remembered-devices-to-suppress-mfa"></a>

If you have selected either **Always** or **User Opt\-In**, you also can suppress MFA challenges on remembered devices for the users of your application\. To use this feature, you must enable MFA for your user pool\. For more information, see [Adding multi\-factor authentication \(MFA\) to a user pool](user-pool-settings-mfa.md)\.

**Note**  
If the device remembering feature is set to **Always** and **Do you want to use a remembered device to suppress the second factor during multi\-factor authentication \(MFA\)?** is set to **Yes**, then the MFA settings for medium/high risks in risk\-based MFA are ignored\. 