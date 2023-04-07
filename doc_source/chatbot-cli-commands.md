# Running AWS CLI commands from chat channels<a name="chatbot-cli-commands"></a>

You can run commands using AWS CLI syntax directly in chat channels\. AWS Chatbot enables you to retrieve diagnostic information, configure AWS resources, and run workflows\. 

When you interact with AWS Chatbot in your chat channels, it prompts you for any missing parameters before it runs the command\. 

**Topics**
+ [Required permissions](#required-permissions)
+ [Non\-supported operations](#forbidden-permissions)
+ [Using commands](#intro-to-the-aws-cli-in-slack)
+ [Running commands](#Things-to-know-about-cli)
+ [Configuring commands support on an existing chat channel](#setting-up-aws-cli-on-slack)
+ [Enabling multiple accounts to use commands](#multiple-accounts-in-a-channel)

## Required permissions<a name="required-permissions"></a>

To perform actions in your chat channels, you must first have the appropriate permissions\. For more information about AWS Chatbot's permissions, see [Understanding permissions](understanding-permissions.md)\.

## Non\-supported operations<a name="forbidden-permissions"></a>

AWS Chatbot does not support running commands for the following operations:
+  Chatbot 
  + All Operations
+  Amazon Cognito user pools 
  + All Operations
+  IAM 
  + All Operations
+  AWS Key Management Service 
  + All Operations
+  Amazon SimpleDB 
  + All Operations
+  Secrets Manager 
  + All Operations
+  AWS IAM Identity Center \(successor to AWS Single Sign\-On\) 
  + All Operations
+ AWS Security Token Service
  +  All Operations 
+ AWS AppSync
  +  ListApiKeys 
+  AWS CodeCommit 
  + GetFile
  + GetCommit
  + GetDifferences
+ Amazon Connect
  + GetFederationToken
+ Amazon DynamoDB
  + BatchGetItem
  + GetItem
+ Amazon EC2
  + GetPasswordData
+ Amazon ECR
  + GetAuthorizationToken
  + GetLogin
+ Amazon GameLift
  + RequestUploadCredentials
  + GetInstanceAccess
+ Lightsail
  + DownloadDefaultKeyPair
  + GetInstanceAccessDetail
  + GetKeyPair
  + GetKeyPairs
  + UpdateRelationalDatabase
+ Amazon Redshift
  + GetClusterCredentials
+ StorageGateway
  + DescribeChapCredentials
+ Amazon S3
  + GetBucketPolicy
  + GetObject
  + HeadObject
+ Snowball
  + GetJobUnlockCode

## Using commands<a name="intro-to-the-aws-cli-in-slack"></a>

After you set up the AWS Chatbot, you run commands with the following prefix:

`@aws`

**Note**  
If you are using Slack and AWS is not listed as a valid member of the channel, you need to add the AWS Chatbot app to the Slack workspace and invite it to the channel\. For more information, see the [Getting started guide for AWS Chatbot](getting-started.md)\.

The AWS Chatbot command syntax is the same as you would use in a terminal:

`@aws service command --options`

**Note**  
You can specify parameters with either a double hyphen \(*\-\-option*\) or a single hyphen \(*\-option*\)\. This allows you to use a mobile device to run commands without running into issues with the mobile device automatically converting a double hyphen to a long dash\.

**Note**  
AWS CLI commands run from AWS Chatbot have an execution [timeout](https://docs.aws.amazon.com/whitepapers/latest/serverless-architectures-lambda/timeout.html) of 15 seconds\. If a command response is not received within 15 seconds, you receive a timeout error message\. If you have longer running jobs, such as AWS Lambda functions, you should invoke them asynchronously from AWS Chatbot\. The maximum allowable Lambda function execution timeout is 900 seconds \(15 minutes\)\. For more information about asynchronous invocation, see [Asynchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html) in the *AWS Lambda Developer Guide*\.

For example, enter the following read\-only command to view a list of your Lambda functions:

`@aws lambda list-functions`

Enter the following commands to list and chart CloudWatch alarms:

`@aws cloudwatch describe-alarms --state ALARM`

You can also use CLI commands to change you AWS resources\. For example, enter the following command to change your Kinesis shards:

`@aws kinesis update-shard-count --stream-name samplestream --scaling-type UNIFORM_SCALING --target-shard-count 6 `

You can enter a complete AWS CLI command with all the parameters, or you can enter the command without parameters and AWS Chatbot prompts you for missing parameters\.

For more information on commonly used CLI commands, see [Using CLI commands with AWS Chatbot \- Common use cases](common-use-cases.md)\. For an exhaustive list of CLI commands, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/index.html)\.

**Note**  
If you find you are unable to run commands, you may need to switch your user role or contact your administrator to find out what actions are permissible\.

The following limitations apply to running AWS CLI commands in your chat rooms:
+ You may experience some latency when invoking commands through AWS Chatbot\.
+ Regardless of their AWS Chatbot role permissions, users cannot run IAM, AWS Security Token Service, or AWS Key Management Service commands within chat channels\.
+ Amazon S3 service commands support Linux\-style command aliases such as **ls** and **cp**\. AWS Chatbot does not support Amazon S3 command aliases for commands in Slack\.
+ Users cannot display or decrypt secret keys or key pairs for any AWS service, or pass IAM credentials\.
+ You can't use AWS CLI command memory \(that is, recent commands appear when the user presses up arrow or down arrow keys\) in the chat channel\. You must enter, or copy and paste each AWS CLI command in the chat channel\.
+ You can create AWS support cases through your chat channels\. You cannot add attachments to these cases from the chat channel\.
+ Chat channels do not support standard AWS CLI pagination\. 

## Running commands<a name="Things-to-know-about-cli"></a>

AWS Chatbot tracks your use of command options and prompts you for any missing parameters before it runs the command you want\. 

For example, if you enter `@aws lambda get-function` with no further arguments, the Chatbot requests the function name\. Then, run the `@aws lambda list-functions` command, find the function name you need, and re\-run the first command with the corrected option\. Add more parameters for the initial command with `@aws function-name name`\. AWS Chatbot parses your commands and helps you complete the correct syntax so it can run the complete AWS CLI command\.

### Getting help for AWS services<a name="getting-help-in-the-chat-window"></a>

To get help about commands for any AWS service, enter **@aws** followed by the service name, as shown following: 

`@aws lambda --help`

`@aws cloudwatch describe-alarms --help`

### Formatting data and viewing logs<a name="formatting-in-the-chat-window.title"></a>

To ensure data from Amazon CloudWatch alarms is correctly formatted, attach the **Lambda\-Invoke Command Permissions** and **ReadOnly Commands Permissions** IAM policies to the role in the AWS Chatbot console for users in the chat channel\. 

Run the `cloudwatch describe-alarms` command to show CloudWatch alarms in chart form as follows: 

`@aws cloudwatch describe-alarms `

You can change the command to only include notifications in the alarm state, filtering out other notifications, by adding the following option:

`@aws cloudwatch describe-alarms --state ALARM`

To see alarms from a different AWS Region, include that Region in the command:

`@aws cloudwatch describe-alarms --state ALARM --region us-east-1`

You can also filter AWS CLI output by using the optional `query` parameter\. A query uses JMESPath syntax to create an expression to filter your output to your specifications\. For more information about filtering, see [Filtering AWS CLI output](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-filter.html) in the *AWS Command Line Interface User Guide*\. For more information about JMESPath syntax, see [their website](https://jmespath.org/)\. The following example shows how to limit AWS CLI output for the `cloudwatch describe-alarms` command to just the alarm name, description, state, and reason attributes\.

```
@AWS cloudwatch describe-alarms --query 
 @.{MetricAlarms:MetricAlarms[*].
 {AlarmName:AlarmName, AlarmDescription:AlarmDescription, StateValue:StateValue, 
 StateReason:StateReason, Namespace:Namespace, MetricName:MetricName, 
 Dimensions:Dimensions, ComparisonOperator:ComparisonOperator, Threshold:Threshold, 
 Period:Period, EvaluationPeriods:EvaluationPeriods, Statistic:Statistic}} 
 --region us-east-2
```

### Displaying Amazon CloudWatch Logs information<a name="logs-in-the-chat-window.title"></a>

CloudWatch alarm notifications show buttons in chat client notifications to view logs related to the alarm\. These notifications use the [CloudWatch Log Insights feature](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\. There may be service charges for using this feature to query and show logs\.

You can view CloudWatch logs, including error logs, that are associated with the CloudWatch alarm by choosing **Show logs** at the bottom of the alarm notification\. AWS Chatbot displays the first 30 log entries from the start of the alarm evaluation period\. AWS Chatbot uses CloudWatch Log Insights to query for logs\. The query results contain a link to the CloudWatch Log Insights console, where a user can dive deeper into logs details\.

Choose **Show error logs** to filter search results to log entries containing Error, Exception, or Fail terms\.

The log shows a command that a user can copy, paste, and edit to re\-run the query for viewing logs\.

### Creating an AWS Support case<a name="create-a-support-case"></a>

The **AWS Support Command Permissions** policy appears in the AWS Chatbot console when you configure resources\. It's provided in the AWS Chatbot console so that you can set up new roles for users in your chat client to create AWS support tickets through their chat channels\. 

You can quickly create a new AWS support case by entering the following:

`@aws support create-case`

Follow the prompts from AWS Chatbot to fill out the support case with its needed parameters\. When you complete the case information entry, AWS Chatbot asks for confirmation\. You will not be able to use file attachments\.

For any AWS Chatbot role that creates AWS Support cases, you need to attach the **AWS Support command permissions** policy to the role\. For existing roles, you will need to attach the policy in the IAM console\.

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

## Configuring commands support on an existing chat channel<a name="setting-up-aws-cli-on-slack"></a>

If you have existing chat channels using the AWS Chatbot, you can reconfigure them in a few steps to support the AWS CLI\.

1. [Open the AWS Chatbot console](https://us-east-2.console.aws.amazon.com/chatbot/home?region=us-east-2#/chat-clients)\.

1. In the **Configured Clients** page, select the chat client\. If you have only one, its contents \(the list of chat channels\) appear on the page\.
**Note**  
In this procedure, we assume use of an existing AWS Chatbot chat channel configuration\. The process is very similar if you need to create a new chat client configuration by choosing **Configure new client**\.

1. Choose a channel from the **Configured channels** list, and choose **Edit**\. The selected channel can be public or private\.

1. Define your **Role setting** by choosing a **Channel role** or **User roles**\. For more information about role types, see [Role setting](understanding-permissions.md#role-settings):

------
#### [ Channel role ]

   1. For **Role setting**, choose **Channel role**\.

   1. For **Channel role**, choose **Create new role**\. If you want to use an existing role instead, choose **Use an existing role**\. To use an existing IAM role, you will need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9, \.\\w\+=,\.@\-\_\.

   1. For **Role policy template**, choose **Read Only command permissions** and **Lambda\-Invoke command permissions**\.
**Note**  
If you plan to have users of the role submit AWS Support cases, also attach the **AWS Support command permissions** policy\.
If you want the role to allow users to manage incidents, add the **Incident Manager Permissions** policy\.

------
#### [ User roles ]

   1. For **Role setting**, choose **User roles**\.

------

1. Select the policies that will make up your [channel guardrail policies](understanding-permissions.md#channel-guardrails)\. Your channel guardrail policies control what actions are available to your channel members\.
**Note**  
If you initially had permission to run Lambda invoke, it is contained in **All actions permitted**\.
**Note**  
To run most CLI commands from your Slack channel, ensure you select **All actions permitted**\.
**Note**  
You do not need to edit or change the Amazon SNS topics configuration for the chat channel\.

1. Choose **Save**\.

   You can use the IAM console to modify an existing IAM role\. By simply attaching the three additional AWS Chatbot policies to the IAM role, users of that role can immediately begin using commands in the chat channel\. To do so, see [Configuring an IAM Role for AWS Chatbot](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

**Important**  
If you have a large number of chat channels and you want to have the same command permissions across multiple channels, you can apply the configured AWS Chatbot role to any of your other chat channels without further modification\. The IAM policies will be consistent across chat channels that support commands in your AWS Chatbot service\.

## Enabling multiple accounts to use commands<a name="multiple-accounts-in-a-channel"></a>

You can configure AWS Chatbot for multiple AWS accounts in the same chat channel\. When you work with AWS Chatbot for the first time in that channel, it will ask you which account you want to use\. AWS Chatbot will remember the account selection for 7 days\.

To change the default account in the channel, enter `@aws set default-account` and select the account from the list\.