# Creating the CloudWatch Logs IAM role<a name="cognito-user-pools-using-import-tool-cli-cloudwatch-iam-role"></a>

If you're using the Amazon Cognito CLI or API, then you need to create a CloudWatch IAM role\. The following procedure describes how to create an IAM role that Amazon Cognito can use to write the results of your import job to CloudWatch Logs\. 

**Note**  
When you create an import job in the Amazon Cognito console, you can create the IAM role at the same time\. When you choose to **Create a new IAM role**, Amazon Cognito automatically applies the appropriate trust policy and IAM policy to the role\.

**To create the CloudWatch Logs IAM role for user pool import \(AWS CLI, API\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Create a new IAM role for an AWS service\. For detailed instructions, see [Creating a role for an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *AWS Identity and Access Management User Guide*\.

   1. When you select a **Use case** for your **Trusted entity type**, choose any service\. Amazon Cognito isn't currently listed in service use cases\.

   1. In the **Add permissions** screen, choose **Create policy** and insert the following policy statement\. Replace *REGION* with the AWS Region of your user pool, for example `us-east-1`\. Replace *ACCOUNT* with your AWS account ID, for example `111122223333`\.

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": [
                      "logs:CreateLogGroup",
                      "logs:CreateLogStream",
                      "logs:DescribeLogStreams",
                      "logs:PutLogEvents"
                  ],
                  "Resource": [
                      "arn:aws:logs:REGION:ACCOUNT:log-group:/aws/cognito/*"
                  ]
              }
          ]    
      }
      ```

1. Because you didn't choose Amazon Cognito as the trusted entity when you created the role, you now must manually edit the trust relationship of the role\. Choose **Roles** from navigation pane of the IAM console, then choose the new role that you created\.

1. Choose the **Trust relationships** tab\.

1. Choose **Edit trust policy**\.

1. Paste the following policy statement into **Edit trust policy**, replacing any existing text: 

   ```
   {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Effect": "Allow",
                   "Principal": {
                       "Service": "cognito-idp.amazonaws.com"
                   },
                   "Action": "sts:AssumeRole"
               }
           ]
       }
   ```

1. Choose **Update policy**\. 

1. Note the role ARN\. You'll provide the ARN when you create your import job\.