--------

AWS Chatbot is in beta and is subject to change\.

--------

# Running AWS CLI Commands from Slack Channels<a name="chatbot-cli-commands"></a>

You can run commands using AWS CLI syntax directly in Slack channels for your AWS account\. AWS Chatbot allows you to retrieve diagnostic information, invoke Lambda functions, and create support cases for your AWS resources\. 

When you interact with AWS Chatbot in Slack, it parses your input and prompts you for any missing parameters before it runs the command\. 

## How to Use Commands in Slack<a name="intro-to-the-aws-cli-in-slack"></a>

After you set up the AWS Chatbot in your Slack workspace, you run commands in Slack with the following prefix:

`@aws`

The AWS Chatbot command syntax is the same as you would use in a terminal:

`@aws service command --options`

For example, enter a read\-only command to view a list of your Lambda functions:

`@aws lambda list-functions`

Enter commands to list and chart CloudWatch alarms:

`@aws cloudwatch describe-alarms --state ALARM`

You can enter a complete AWS CLI command with all the parameters, or you can enter the command without parameters and AWS Chatbot prompts you for missing parameters\.

Some limitations apply to running AWS CLI commands in your Slack chat rooms:
+ AWS Chatbot does not support commands to create, delete, or configure AWS resources \(for example, to delete an S3 bucket\)\. 
+ You may experience some latency when invoking commands through AWS Chatbot\.
+ Regardless of their AWS Chatbot role permissions, users cannot run IAM, AWS STS, or AWS KMS commands within Slack channels\.
+ Amazon S3 service commands support Linux\-style command aliases such as **ls** and **cp**\. AWS Chatbot does not support Amazon S3 command aliases for commands in Slack\.
+ Commands users cannot display or decrypt secret keys or key pairs for any AWS service, or pass IAM credentials\.
+ You can't use AWS CLI command memory \(that is, recent commands appear when the user presses up arrow or down arrow keys\) in the Slack channel\. You must enter, or copy and paste each AWS CLI command\.
+ You can create AWS support cases through your Slack channels\. You cannot add attachments to these cases from the &SLC; channel\.
+ Slack channels do not support standard AWS CLI pagination\. 

## Managing Permissions for Running Commands Using AWS Chatbot<a name="iam-policies-for-slack-channels-cli-support"></a>

With AWS IAM, you can use *identity\-based policies*, which are JSON permissions policy documents, and attach them to an *identity*, such as an IAM user, role, or group\. These policies control what actions that identity can perform\. AWS Chatbot provides three service\-specific pre\-defined IAM policies, available in the AWS Chatbot console, that you can use to quickly set up commands for Slack channels:
+ **ReadOnly Command Permissions**
+ **Lambda\-Invoke Command Permissions**
+ **AWS Support Command Permissions**

You can use any or all of these policies, based on your organization's requirements\. To use them, you need to attach the policies to the IAM roles that AWS Chatbot uses on behalf of users in Slack channels\. The pre\-defined IAM policies simplify AWS Chatbot role configuration and enable you to set up quickly\. 

You can use these IAM policies as templates to define your own policies\. For example, all default policies described here use a wildcard \("\*"\) to apply the policy's permissions to all resources:

```
               "Resource": [
                "*"
            ]
```

You can define custom policies to restrict actions to specific resources in your AWS account\. For more information on specifying resources in a policy, see the section [IAM JSON Policy Elements: Resource](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_resource.html) in the *AWS Identity and Access Management User's Guide*\.

### Using the AWS Chatbot Read\-Only Command Permissions Policy<a name="about-readonlycommand-chatbot-policy"></a>

**Note**  
AWS Chatbot does not allow users to run any of the IAM\-specific AWS CLI operations in Slack\.

AWS Chatbot allows operations through the AWS managed policy **ReadOnlyAccess** policy, which provides read\-only access to all AWS services and resources\. The **ReadOnlyAccess** policy is automatically attached for AWS Chatbot\-configured roles in IAM\. The **ReadOnlyAccess** policy permissions apply along with those defined in the AWS Chatbot **ReadOnly Command Permissions** policy\.

The AWS Chatbot **ReadOnly Command Permissions** policy controls access to key AWS services, including IAM, AWS KMS, AWS KMS, and Amazon S3\. When you use the **ReadOnly Command Permissions** policy, you allow or deny the following permissions to users who run commands in Slack: 
+ IAM \(Deny All\)
+ AWS KMS \(Deny All\)
+ AWS STS \(Deny All\)
+ Amazon Cognito \(allows Read\-Only, denies `GetSigningCertificate` commands\)
+ Amazon EC2 \(allows Read\-Only, denies `GetPasswordData` commands \)
+ Amazon ECR \(allows Read\-Only, denies `GetAuthorizationToken` commands\)
+ GameLift \(allows Read\-Only, denies requests for credentials and `GetInstanceAccess` commands\)
+ Amazon Lightsail \(allows List, Read, denies several key pair operations and `GetInstanceAccess`\)
+ Amazon Redshift \(denies `GetClusterCredentials` commands\)
+ Amazon S3 \(allows Read\-Only commands, denies `GetBucketPolicy` commands\)
+ AWS Storage Gateway \(allows Read\-Only, denies `DescribeChapCredentials` commands\)

The **ReadOnly Command Permissions** policy JSON code is shown following:

```
{
   "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "iam:*",
                "kms:*",
                "sts:*",
                "cognito-idp:GetSigningCertificate",
                "ec2:GetPasswordData",
                "ecr:GetAuthorizationToken",
                "gamelift:RequestUploadCredentials",
                "gamelift:GetInstanceAccess",
                "lightsail:DownloadDefaultKeyPair",
                "lightsail:GetInstanceAccessDetail",
                "lightsail:GetKeyPair",
                "lightsail:GetKeyPairs",
                "redshift:GetClusterCredentials",
                "s3:GetBucketPolicy",
                "storagegateway:DescribeChapCredentials"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

### Using the AWS Chatbot Lambda\-Invoke Policy<a name="about-lambda-invoke-chatbot-policy"></a>

The AWS Chatbot **Lambda\-Invoke Command Permissions** policy allows users to invoke AWS Lambda functions in Slack channels\. 

 By default, invoked Lambda functions can perform *any operation*\. You might need to define a more restrictive inline IAM policy that allows permissions to invoke specific Lambda functions, such as functions specifically developed for your DevOps team that only they should be able to invoke, and deny permissions to invoke Lambda functions for any other purpose\.

The **Lambda\-Invoke Command Permissions** policy is shown following:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "lambda:invokeAsync",
                "lambda:invokeFunction"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

You can use the two AWS Chatbot policies as templates to define your own policies with different sets of permissions, and more restrictive permissions, than the permissions supported by the user\-managed policies\. Always follow the IAM practice of granting only the permissions required for your users to do their jobs\.

## Running Commands in Slack<a name="Things-to-know-about-cli-in-Slack"></a>

AWS Chatbot tracks your use of command options and prompts you for any missing parameters before it runs the command you want\. 

For example, if you enter `@aws lambda get-function` with no further arguments, the Chatbot requests the function name\. Then, you could run the `@aws lambda list-functions` command, find the function name you need, and re\-run the first command with the corrected option\. Continue providing parameters for the initial command with `@aws function-name name`\. AWS Chatbot parses your commands and helps you complete the correct syntax so it can run the initial AWS CLI command\.

### Getting help for AWS Services<a name="getting-help-in-the-chat-window"></a>

To get help about commands for any AWS service, enter **@aws** followed by the service name, as shown following: 

`@aws lambda --help`

`@aws cloudwatch describe-alarms --help`

### Formatting Data and Viewing Logs<a name="formatting-in-the-chat-window.title"></a>

To ensure data from Amazon CloudWatch alarms appears correctly formatted, ensure that the **Lambda\-Invoke Command Permissions** and **ReadOnly Commands Permissions** IAM policies are configured in the AWS Chatbot console for the role in the Slack channel\. 

You can use the `cloudwatch describe-alarms` command to show CloudWatch alarms in chart form: 

`@aws cloudwatch describe-alarms `

You can change the command to only include notifications in the alarm state, by adding the following option:

`@aws cloudwatch describe-alarms --state ALARM`

If you want to see alarms from a different AWS Region, include that Region in the command the first time you specify it:

`@aws cloudwatch describe-alarms --state ALARM --region us-east-1`

### Displaying CloudWatch Logs Information<a name="logs-in-the-chat-window.title"></a>

CloudWatch alarm notifications will show buttons in Slack to show logs related to the reported alarm\. These notifications use the CloudWatch Log Insights feature\. There may be service charges for using this feature to query and show logs\.

You can view CloudWatch logs, including error logs, directly associated with the CloudWatch alarm by choosing **Show logs** at the bottom of the alarm notification in Slack\. AWS Chatbot displays the first 30 log entries starting from the beginning of the alarm evaluation period\. AWS Chatbot uses CloudWatch Log Insights to query for logs\. The query results contain a link to the CloudWatch Log Insights console, where a user can dive deeper into logs details\.

Choose **Show error logs** to filter search results to log entries containing Error, Exception or Fail terms\.

The log shows a command that a user can copy, paste\. and edit to re\-run the query for viewing logs in Slack\.

### Creating an AWS Support Case<a name="create-a-support-case"></a>

You can quickly create a new AWS support case from Slack by entering the following:

`@aws support create-case`

Follow the prompts from AWS Chatbot to fill out the support case with its needed parameters\. When you complete the case information entry, AWS Chatbot asks you to confirm case creation\. You will not be able to use file attachments\.

For any AWS Chatbot role that will create AWS Support cases, you need to attach the **AWS Support command permissions** policy to the role\. For existing roles, you will need to attach the policy in the IAM console\.

The **AWS Support Command Permissions** policy appears in the AWS Chatbot console when you configure resources\. It's provided in the AWS Chatbot console so that you can set up new roles for users in Slack to create AWS support tickets through their Slack channels\. 

In the IAM console, this policy appears as **AWSSupportAccess**\. 

It is an AWS managed policy\. Attach this policy in IAM to any role for AWS Chatbot usage\. You can define your own policy with greater restrictions, using this policy as a template\.

The **Support Command Permissions** policy applies only to the AWS Support service\.

The policy's JSON code is shown following:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "support:*"
            ],
            "Resource": "*"
        }
    ]
}
```

## Configuring Commands Support on an Existing Slack Channel<a name="setting-up-aws-cli-on-slack"></a>

If you have existing Slack channels using the AWS Chatbot, you can reconfigure them in a few steps to support the AWS CLI\.

1. [Log in to the AWS Chatbot console](https://us-east-2.console.aws.amazon.com/chatbot/home?region=us-east-2#/chat-clients)\.

1. In the **Configured Clients** page, select the **Slack** workspace\. If you have only one workspace, its contents \(the list of Slack channels\) appear on the page\.
**Note**  
In this procedure, we assume use of an existing AWS Chatbot Slack channel configuration\. The process is very similar if you need to create a new Slack configuration by choosing **Configure new client**\.

1. Choose a Slack channel from the **Configured channels** list, and choose **Configure channel**\. The selected channel can be public or private\.

1. In the **Configure Slack channel** page, define the **IAM permissions** that the chatbot uses for messaging your Slack chat room:

1. For **Role**, choose **Create a new role from template**\.
**Note**  
The first time you reconfigure a Slack channel to work with commands, you must create a new role using the AWS Chatbot's policies as templates, or choose an existing role that provides the permissions you need\. 

1. For **Policy templates**, choose the **Read Only command permissions** and **Lambda\-Invoke command permissions** policies for the role\.

   If you plan to have users of the role submit AWS Support cases, also attach the **AWS Support command permissions** policy\.

1. If you want the role to allow users to view CloudWatch alarms in graphical format, add the **Notification Permissions** policy\. It provides the necessary Read and List permissions for CloudWatch alarms, events and logs, and for Amazon SNS topics\.

1. For the **Role name**, enter a new role name using alphanumeric characters\. Valid characters: a\-z, A\-Z, 0\-9, \.\\w\+=,\.@\-\_\.
**Note**  
You do not need to edit or change the Amazon SNS topics configuration for the Slack channel\.

1. Choose **Save**\.

   You can use the IAM console to modify an existing IAM role\. By simply attaching the three additional AWS Chatbot policies to the IAM role, users of that role can immediately begin using commands in the Slack channel\. To do so, see [Configuring an IAM Role for AWS Chatbot](getting-started.md#AWS::Chatbot::Role)\.</para>

**Important**  
If you have a large number of Slack channels and you want to have the same command permissions across multiple channels, you can apply the configured AWS Chatbot role from that channel to any of your other Slack channels without further modification\. The IAM policies will be consistent across Slack channels supporting commands in your AWS Chatbot service\.

## Enabling Multiple Accounts to Use Commands in a Slack Channel<a name="multiple-accounts-in-a-channel"></a>

You can configure AWS Chatbot for multiple AWS accounts in the same Slack channel\. When you work with AWS Chatbot for the first time in that channel, it will ask you which account you want to use\. AWS Chatbot will remember the account selection for 7 days\.

To change the default account in the channel, enter `@aws set default-account` and select the account from the list\.