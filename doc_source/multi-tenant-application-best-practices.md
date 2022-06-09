# Multi\-tenant application best practices<a name="multi-tenant-application-best-practices"></a>

You can use Amazon Cognito user pools to secure small multi\-tenant applications where the number of tenants and expected volume match the related Amazon Cognito service quota\.

**Note**  
Amazon Cognito [Quotas](https://docs.aws.amazon.com/cognito/latest/developerguide/limits.html) are applied per AWS account and Region\. These quotas are shared across all tenants in your application\. Review the Amazon Cognito service quotas and make sure that the quota meets the expected volume and the expected number of tenants in your application\.

You have four ways to secure multi\-tenant applications: user pools, application clients, groups, or custom attributes\.

**Topics**
+ [User\-pool\-based multi\-tenancy](bp_user-pool-based-multi-tenancy.md)
+ [App\-client\-based multi\-tenancy](application-client-based-multi-tenancy.md)
+ [Group\-based multi\-tenancy](group-based-multi-tenancy.md)
+ [Custom\-attribute\-based multi\-tenancy](custom-attribute-based-multi-tenancy.md)
+ [Multi\-tenancy security recommendations](multi-tenancy-security-recommendations.md)