# App\-client\-based multi\-tenancy<a name="application-client-based-multi-tenancy"></a>

With application client\-based multi\-tenancy, you can map the same user to multiple tenants without the need to recreate a user’s profile\. You can create an application client for each tenant and make the tenant external IdP the only identity provider that this application client can use\. For more information see, [ Configuring a user pool app client](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-app-idp-settings.html)\. 

The hosted UI sets a session cookie in the browser so that it recognizes a user who has already authenticated\. When you authenticate users with *native accounts* in a user pool with multiple app clients, their session cookie authenticates them for all app clients in the same user pool\. Native accounts are user accounts in the user pool directory that weren't created by federation with a third\-party IdP\. The session cookie is valid for one hour\. You can't change the session cookie duration\. 

You can use app\-client\-based multi\-tenancy in the following scenarios:
+ Your application has the same configurations across all tenants\. For example, data residency and password policy are the same across all tenants\.
+ Your application has a one\-to\-many mapping between the user and tenants\. For example, a single user might have access to multiple tenants using the same profile\.
+ You have a federation\-only multi\-tenant application where tenants always use an external IdP to sign in to your application\.
+ You have a B2B multi\-tenant application, and tenant backend services use a client\-credentials grant to access your services\. In this case, you can create an application client for each tenant and share the client\-id and secret with the tenant backend service for machine\-to\-machine authentication\.

 **Effort level** 

To use this approach, development effort is high\. You must implement tenant\-matching logic and a user interface to match a user to the application client for their tenant\.