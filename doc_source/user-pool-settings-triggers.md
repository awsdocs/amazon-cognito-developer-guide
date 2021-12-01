# Configuring user pool Lambda triggers<a name="user-pool-settings-triggers"></a>

You can use AWS Lambda triggers to customize workflows and the user experience with Amazon Cognito\. You can create the following Lambda triggers: **Pre sign\-up**, **Pre authentication**, **Custom message**, **Post authentication**, **Post confirmation**, **Define Auth Challenge**, **Create Auth Challenge**, **Verify Auth Challenge Response**, and **User Migration**\.

A user migration Lambda trigger allows easy migration of users from your existing user management system into your user pool\.

For examples of each Lambda trigger, see [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.

**Note**  
The **Custom message** AWS Lambda trigger is an advanced way to customize messages for email and SMS\. For more information, see [Customizing user pool workflows with Lambda triggers](cognito-user-identity-pools-working-with-aws-lambda-triggers.md)\.