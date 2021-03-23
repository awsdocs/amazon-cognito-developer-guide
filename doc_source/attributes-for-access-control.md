# Using attributes for access control as a form of attribute\-based access control<a name="attributes-for-access-control"></a>

You can use IAM policies to control access to AWS resources through Amazon Cognito identity pools based on user attributes\. These attributes can be drawn from social and corporate identity providers\. You can map attributes within providers’ access and ID tokens or SAML assertions to tags that can be referenced in the IAM permissions policies\. 

You can choose default mappings or create your own custom mappings in Amazon Cognito identity pools\. The default mappings allow you to write IAM policies based on a fixed set of user attributes\. Custom mappings allow you to select a custom set of user attributes that are referenced in the IAM permissions policies\. The **Attribute names** in the Amazon Cognito console are mapped to **Tag key for principal**, which are the tags that are referenced in the IAM permissions policy\.

For example, let's say that you own a media streaming service with a free and paid membership\. You store the media files in Amazon S3 and tag them with free or premium tags\. You can use attributes for access control to allow access to free and paid content based on user membership level, which is part of user's profile\. You can map the membership attribute to a tag key for principal to be passed on to the IAM permissions policy\. This way you can create a single permissions policy and conditionally allow access to premium content based on the value of membership level and tag on the content files\. 

**Topics**
+ [Using attributes for access control with Amazon Cognito identity pools](using-afac-with-cognito-identity-pools.md)
+ [Using attributes for access control policy example](using-attributes-for-access-control-policy-example.md)
+ [Disable attributes for access control \(console\)](disable-afac.md)
+ [Default provider mappings](provider-mappings.md)

Using attributes to control access has several benefits:
+ Permissions management is easier when you use attributes for access control\. You can create a basic permissions policy that uses user attributes instead of creating multiple policies for different job functions\. 
+ You don't need to update your policies whenever you add or remove resources or users for your application\. The permissions policy will only grant the access to users with the matching user attributes\. For example, you might need to control the access to certain S3 buckets based on the job title of users\. In that case, you can create a permissions policy to allow access to these files only for users within the defined job title\. For more information, see [IAM Tutorial: Use SAML session tags for ABAC](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_abac-saml.html)\.
+ Attributes can be passed as principal tags to a policy that allows or denies permissions based on the values of those attributes\.