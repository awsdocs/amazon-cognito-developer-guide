# Creating the CloudWatch Logs IAM Role \(AWS CLI, API\)<a name="cognito-user-pools-using-import-tool-cli-cloudwatch-iam-role"></a>

If you're using the Amazon Cognito CLI or API, then you need to create a CloudWatch IAM role\. The following procedure describes how to enable Amazon Cognito to record information in CloudWatch Logs about your user pool import job\. 

**Note**  
You don't need to use this procedure if you are using the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home), because the console creates the role for you\.

**To create the CloudWatch Logs IAM Role for user pool import \(AWS CLI, API\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\.

1. For a new role, choose **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\.

1. In **Common use cases**, choose **EC2**\.

1. Choose** Next: Permissions**\.

1. In **Attach permissions policy**, choose **Create policy** to open a new browser tab and create a new policy from scratch\.

1. On the **Create policy** page, choose the **JSON** tab\.

1. In the **JSON** tab, copy and paste the following text as your role access policy, replacing any existing text: 

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

1. Choose **Review policy**\. Add a name and optional description and choose **Create policy**\.

   After you create the policy, close that browser tab and return to your original tab\.

1. In **Attach Policy**, choose **Next: Tags**\.

1. \(Optional\) In **Tags**, add metadata to the role by entering tags as key–value pairs\. 

1. Choose **Next: Review**\.

1. In **Review**, enter a **Role Name**\. 

1. \(Optional\) Enter a **Role Description**\.

1. Choose **Create role**\.

1. In the navigation pane of the IAM console, choose **Roles**\.

1. In **Roles**, choose the role you created\.

1. In **Summary**, choose the **Trust relationships** tab\.

1. In the **Trust relationships** tab, choose **Edit trust relationship**\.

1. Copy and paste the following trust relationship text into the **Policy Document** text box, replacing any existing text: 

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

1. Choose **Update Trust Policy**\. You are now finished creating the role\.

1. Note the role ARN\. You need this later when you're creating an import job\.