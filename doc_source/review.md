# Reviewing Your User Pool Creation Settings<a name="review"></a>

Before you create your user pool, you can review the different settings and edit them in the AWS Management Console\. Amazon Cognito validates the user pool settings and warns you if something needs to be changed\. For example:

**Warning**  
This user pool does not have an IAM role defined to allow Amazon Cognito to send SMS messages, so it will not be able to confirm phone numbers or for MFA after August 31, 2016\. You can define the IAM role by selecting a role on the **Verifications** panel\.

If you see a message, follow the instructions to fix them before choosing **Create pool**\.