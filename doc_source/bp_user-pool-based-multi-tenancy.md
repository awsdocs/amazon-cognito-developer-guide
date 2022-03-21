# User\-pool\-based multi\-tenancy<a name="bp_user-pool-based-multi-tenancy"></a>

With this design, you can create a user pool for each tenant in your application\. This approach provides maximum isolation for each tenant\. You can implement different configurations for each tenant\. Tenant isolation by user pool gives you flexibility in user\-to\-tenant mapping\. You can create multiple profiles for the same user\. However, each user must sign up individually for each tenant they can access\. Using this approach, you can set up hosted UI for each tenant independently and redirect users to their tenant\-specific instance of your application\. You can also use this approach to integrate with backend services like API Gateway\. 

You can use user\-pool\-based multi\-tenancy in the following scenarios: 
+ Your application has different configurations for each tenant\. For example, data residency requirements, password policy, and MFA configurations can be different for each tenant\. 
+ Your application has complex user\-to\-tenant role mapping\. For example, a single user could be a “Student” in tenant A, and the same user could also be a “Teacher” in tenant B\. 
+ Your application uses the default Amazon Cognito hosted UI as the primary way for native users to authenticate\. Native users are those that have been created in the user pool with user name and password\.
+ Your application has a silo multi\-tenant application where each tenant gets a full instance of your application infrastructure for their usage\. 

 **Effort level** 

The development and operation effort to use this approach is high\. You must build tenant onboarding and administration components into your application that uses Amazon Cognito API operations and automation tools\. These components are necessary to create the required resources for each tenant\. You also must implement a tenant\-matching user interface\. In addition, you must add logic to your application so that users can sign up and sign in to their corresponding tenant’s user pool\. 