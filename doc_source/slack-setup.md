# Tutorial: Get started with Slack<a name="slack-setup"></a>

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites-slack)
+ [Step 1: Setting up AWS Chatbot with Slack](#slack-client-setup)
+ [Step 2: Test notifications from AWS services to Slack](#test-notifications-slack)
+ [Next steps](#next-steps-slack)

## Prerequisites<a name="getting-started-prerequisites-slack"></a>

Before you get started, make sure you've completed the tasks in [Setting up AWS Chatbot](getting-started.md#setting-up)\. You will need to choose a permissions scheme in the following procedure\. This scheme determines the permissions your channel members will have and what AWS Chatbot can do on your behalf\. For more information about AWS Chatbot permissions, see [Understanding permissions](understanding-permissions.md)\.

## Step 1: Setting up AWS Chatbot with Slack<a name="slack-client-setup"></a>

### <a name="slack-setupst"></a>

To allow AWS Chatbot to send notifications or run commands in your Slack channel, you must configure AWS Chatbot with Slack\. Workspace administrators must approve the use of the AWS Chatbot app in the workspace\. Any workspace user can configure the workspace to receive notifications or perform actions\.

**To configure a Slack client**

1. Add AWS Chatbot to the Slack workspace:

   1. In Slack, on the left navigation pane, choose **Apps**\.
**Note**  
If you do not see **Apps** in the left navigation pane, choose **More**, then choose **Apps**\.

   1. If AWS Chatbot is not listed, choose the **Browse Apps Directory** button\.

   1. Browse the directory for the AWS Chatbot app and then choose **Add** to add AWS Chatbot to your workspace\.

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Under **Configure a chat client**, choose **Slack**, then choose **Configure client**\.

1. From the dropdown list at the top right, choose the Slack workspace that you want to use with AWS Chatbot\.

   There's no limit to the number of workspaces that you can set up for AWS Chatbot, but you can set up only one at a time\.

1. Choose **Allow**\.

1. On the **Workspace details** page, you can choose to continue within the console or with an AWS CloudFormation template:
   + To use an AWS CloudFormation template, copy and paste the **Workspace ID** found under **Workspace details**\. For more information, see [AWS::Chatbot::SlackChannelConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-chatbot-slackchannelconfiguration.html) in the *AWS CloudFormation User Guide*\.
   + To continue within the console, choose **Configure new channel**\. 

1. Under **Configuration details**, enter a name for your configuration\. The name must be unique across your account and can't be edited later\.

1. If you want to enable logging for this configuration, choose **Publish logs to Amazon CloudWatch Logs**\. For more information, see [Amazon CloudWatch Logs for AWS Chatbot](cloudwatch-logs.md)\.
**Note**  
There is an extra charge for using CloudWatch Logs\.

1. For **Slack channel**, choose the channel that you want to use\.
**Note**  
You can use private Slack channels with AWS Chatbot\. To do so, choose **Private channel**\. In Slack, copy the Channel ID of the private channel by right\-clicking on the channel name in the left pane and choosing **Copy Link**\. The Channel ID is the string at the end of the URL \(for example, `AB3BBLZZ8YY`\)\. In AWS Chatbot, paste the ID into the **Channel URL** field\. \(If you copy the URL of the private Slack channel, the AWS Chatbot console shows only the Channel ID value when you paste it into the field\.\) 
**Note**  
If you configure a private Slack channel, run the `/invite @AWS` command in Slack to invite the AWS Chatbot to the chat room\.

1. Choose your **Role Setting**\.
**Tip**  
 Your role setting dictates what permissions your channel members have\. A channel role gives all members the same permissions\. This is useful if your channel members typically perform the same actions in Slack\. A user role requires your channel members to choose their own roles\. As such, different users in your channels can have different permissions\. This is useful if your channel members are diverse or you don’t want new channel members to perform actions as soon as they join the channel\. For more information, see [Role setting](understanding-permissions.md#role-settings)\. 

------
#### [ Channel role ]

   1. For **Role setting**, choose **Channel role**\.

   1. For **Channel role**, choose **Create new role**\. If you want to use an existing role instead, choose **Use an existing role**\. To use an existing IAM role, you will need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9, \.\\w\+=,\.@\-\_\.

   1. For **Role policy template**, choose the template you wish to use\.

------
#### [ User roles ]

   1. For **Role setting**, choose **User roles**\.

------

1. Select the policies that will make up your [channel guardrails](understanding-permissions.md#channel-guardrails)\. Your channel guardrails control what actions are available to your channel members\.
**Note**  
To run most CLI commands from your Slack channel, ensure you select **All actions permitted**\.

1. Choose your notification settings:

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the Amazon SNS topic for the client subscription\. This topic determines the content that's sent to the Slack channel\. If the region has additional SNS topics, you can choose them from the same dropdown list\.

   1. To add an Amazon SNS topic from another AWS Region to the notification subscription, choose **Add another Region**\.
**Note**  
For a tutorial on subscribing existing Amazon SNS topics to AWS Chatbot, see [Tutorial: Subscribing an Amazon SNS topic to AWS Chatbot](subscribe-sns-topic.md)\.

1. Choose **Save**\. 

1. Add AWS Chatbot to the Slack channel:

   1. In your Slack channel, enter **invite @aws**\.

   1. Choose **Invite Them**\.

**Note**  
You can configure a Slack channel to run commands to your AWS account\. For more information, see [Running AWS CLI commands from chat channels](chatbot-cli-commands.md)\.

Notifications from supported services that publish to the chosen Amazon SNS topics will now appear in the Slack channel\.

You can configure as many channels with as many topics as you need\.

The SNS topics you choose also must be configured in the services for which you want to receive notifications\. For more information, see [Monitoring AWS services using AWS Chatbot](related-services.md)\.

If you want to allow AWS Chatbot to answer questions about your AWS resources, turn on AWS Resource Explorer in the Resource Explorer Console\. For more information, see [Getting started with Resource Explorer ](https://docs.aws.amazon.com/resource-explorer/latest/userguide/getting-started.html)in the *AWS Resource Explorer User Guide*\.

## Step 2: Test notifications from AWS services to Slack<a name="test-notifications-slack"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Slack channel, you can test your setup by sending a notification\. To test your notifications, ensure your topics are assigned to a service supported by AWS Chatbot\. For a list of supported services, see [Monitoring AWS services using AWS Chatbot](related-services.md)\. You can also test notifications by using CloudWatch\. For more information, see [Test notifications from AWS services to Amazon Chime or Slack using CloudWatch](test-notifications-cw.md)\.

**Testing notifications with configured clients**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client you want to test\.

1. In the configured client, choose the channel to send a test notification to\.

1. Choose **Send test message**\.

1. View the confirmation message at the top of the screen that shows a message was sent to your Amazon SNS topic\.

1. Confirm the test message in your Slack channel\.

## Next steps<a name="next-steps-slack"></a>

After you configure your chat clients and test that your notifications are working, you might want to explore some of the following topics:
+ Learn about which other AWS services you can integrate with AWS Chatbot in [Monitoring AWS services using AWS Chatbot](related-services.md)\.
+ Learn about what actions you can perform using AWS Chatbot in [Performing actions](performing-actions.md)\.