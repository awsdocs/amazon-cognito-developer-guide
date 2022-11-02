# Adding resource servers for your user pool<a name="cognito-user-pools-resource-servers"></a>

A *resource server* is a server for access\-protected resources\. It handles authenticated requests from an app that has an access token\. A *scope* is a level of access that an app can request to a resource\.

------
#### [ Original console ]

**Note**  
The **Resource Servers** tab appears only when you're editing an existing user pool\.

In the **Resource Servers** tab, you can define custom resource servers and scopes for your user pool\. For more information, see [Defining resource servers for your user pool](cognito-user-pools-define-resource-servers.md)\.

**To define a custom resource server**

1. Choose **Add a resource server**\.

1. Enter the name of your resource server, for example, `Photo Server`\.

1. Enter the identifier of your resource server, for example, `com.example.photos`\.

1. Enter the names of the custom scopes for your resources, such as `read` and `write`\.

1. For each of the scope names, enter a description, such as `view your photos` and `update your photos`\.

Each of the custom scopes that you define appears on the **App client settings** tab, under **OAuth2\.0 Allowed Custom Scopes**; for example `com.example.photos/read`\.

------
#### [ New console ]

**Note**  
You can add **Resource servers** to your user pool after you create it\. Resource server configuration is not included in the new user pool wizard\.

In the **App integration** tab, locate **Resource servers**, where you can define custom resource servers and scopes for your user pool\. For more information, see [Defining resource servers for your user pool](cognito-user-pools-define-resource-servers.md)\.

**To define a custom resource server**

1. Choose **Create a resource server**\.

1. Enter a **Resource server name**, for example, `Photo Server`\.

1. Enter a **Resource server identifier**, for example, `com.example.photos`\.

1. Choose **Add a custom scope** and enter a **Scope name**, such as `read` and `write`\.

1. For each **Scope name**, enter a **Description**, such as `view your photos` and `update your photos`\.

Each of the custom scopes that you define appears on the **App integration** tab under **Resource servers**\. Hover over the number in the **Custom scopes** column to display all scopes and descriptions associate with the resource server\.

------