# Tutorial: Getting started with Slack<a name="slack-setupt"></a>

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites-slackt)
+ [Step 1: Setting up AWS Chatbot with Slack](#slack-client-setupt)
+ [Step 2: Subscribe an Amazon SNS topic to AWS Chatbot](#subscribe-sns-topic-slackt)
+ [Step 3: Test notifications from AWS services to Amazon Chime or Slack](#test-notifications-slackt)
+ [Next steps](#next-steps-slackt)

### Prerequisites<a name="getting-started-prerequisites-slackt"></a>

Before you get started, make sure you've completed the tasks in [Setting up AWS Chatbot](getting-started.md#setting-up)\. You will need to choose a permissions scheme in the following procedure\. This scheme determines the permissions your channel members will have and what AWS Chatbot can do on your behalf\. For more information about AWS Chatbot permissions, see [Understanding permissions](understanding-permissions.md)\.

### Step 1: Setting up AWS Chatbot with Slack<a name="slack-client-setupt"></a>

You use the AWS Chatbot console to configure Slack clients to receive notifications from Amazon Simple Notification Service \(Amazon SNS\) topics and perform actions\.

**Note**  
When you configure your clients, don't enable the **Enable raw message delivery** feature for any Amazon SNS topic subscription that you want to use for AWS Chatbot\. 

AWS Chatbot requires an AWS Identity and Access Management \(IAM\) role with Amazon CloudWatch read permissions and a trust policy that allows AWS Chatbot to use those permissions on your behalf\. When you configure AWS Chatbot, you can create a role with a predefined set of policies to display CloudWatch charts in AWS Chatbot notifications\. 

You can also use an existing IAM role that you can configure for use with AWS Chatbot\. For more information, see [Configuring an IAM role for AWS Chatbot](understanding-permissions.md#editing-iam-roles-for-chatbot)\. For simplicity, particularly in testing your setup, we recommend using the IAM role with predefined policies that you can configure in AWS Chatbot\. 

#### Setting up AWS Chatbot with Slack<a name="slack-setupst"></a>

To allow AWS Chatbot to send notifications to your Slack channel, you must configure AWS Chatbot with Slack\. Owners of Slack workspaces can approve the use of the AWS Chatbot, and any workspace user can configure the workspace to receive notifications or run commands\.

**To configure a Slack client**

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

1. Choose your **Role Setting**\. You can choose a channel IAM role or user roles\. A channel IAM role allows channel members to share the same permissions\. User roles require your channel members to choose their own roles\. If you choose to use a channel IAM role, your users can still choose to use their own user roles\. For more information about role setting, see [Role setting](understanding-permissions.md#role-settings)\.

------
#### [ Channel IAM role ]

   1. For **Role setting**, choose **Channel IAM role**\.

   1. For **Channel IAM role**, choose **Create new role**\. If you want to use an existing role instead, choose **Use an existing role**\. To use an existing IAM role, you will need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9, \.\\w\+=,\.@\-\_\.

   1. For **Role policy template**, choose the template you wish to use\.

------
#### [ User roles ]

   1. For **Role setting**, choose **User roles**\.

------

1. Select the policies that will make up your [channel guardrails](understanding-permissions.md#channel-guardrails)\. Your channel guardrails control what actions are available to your channel members\.
**Note**  
If you initially had permission to run Lambda invoke, it is contained in **All actions permitted**\.
**Note**  
To run most CLI commands from your Slack channel, ensure you select **All actions permitted**\.

1. Choose your notification settings:

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the Amazon SNS topic for the client subscription\. This topic determines the content that's sent to the Slack channel\. If the region has additional SNS topics, you can choose them from the same dropdown list\.

   1. To add an Amazon SNS topic from another AWS Region to the notification subscription, choose **Add another Region**\. 

1. Choose **Save**\. 

1. Add AWS Chatbot to the Slack workspace:

   1. In Slack, on the left navigation pane, choose **Apps**\.
**Note**  
If you do not see **Apps** in the left navigation pane, choose **More**, then choose **Apps**\.

   1. If AWS Chatbot is not listed, choose the **Browse Apps Directory** button\.

   1. Browse the directory for the AWS Chatbot app and then choose **Add** to add AWS Chatbot to your workspace\.

**Note**  
You can configure a Slack channel to run commands to your AWS account\. For more information, see [Running AWS CLI Commands from Slack Channels](chatbot-cli-commands.md)\.

Notifications from supported services that publish to the chosen Amazon SNS topics will now appear in the Slack channel\.

You can configure as many channels with as many topics as you need\.

**Note**  
If you configure a private Slack channel, run the `/invite @AWS` command in Slack to invite the AWS Chatbot to the chat room\.

The SNS topics you choose also must be configured in the services for which you want to receive notifications\. For more information, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

### Step 2: Subscribe an Amazon SNS topic to AWS Chatbot<a name="subscribe-sns-topic-slackt"></a>

You can quickly subscribe existing Amazon SNS topics to the AWS Chatbot service\. You associate the new subscriptions to a Slack channel or Amazon Chime webhook\. After doing so, the messages from those topics will appear in the Slack or Amazon Chime chat rooms\. The Amazon SNS topics must be associated with AWS services that AWS Chatbot supports, and may also require further configuration, such as association with a CloudWatch rule\. This procedure is most useful if you have SNS topics that are already doing significant work with CloudWatch Events and CloudWatch alarms in AWS cloud services supported by AWS Chatbot\.

**Note**  
You can set up each supported AWS service to *target* one or more SNS topics to send notifications to AWS Chatbot\. You do this using each AWS service's console, or using AWS CloudFormation\. If you already have Amazon SNS topics set as targets for supported services, you can configure AWS Chatbot to use those topics\. Notifications from subscribed topics will automatically appear in your Slack or Amazon Chime clients without further configuration\. 

**Note**  
If your SNS topic is encrypted, you must add a section to your AWS KMS key policy to give the sending service permissions to post events to the encrypted SNS topics\. For more information, see [Setting up Amazon SNS topics](getting-started.md#chatbot-sns)\.

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Under **Configured clients**, choose Slack or Amazon Chime\. 

1. Choose any channel in the Slack workspace configuration or webhook in the Amazon Chime webhooks list\.

1. Choose **Edit**\. The configuration page for the channel or webhook appears\. Note that the **Region** Notifications is already configured\.

1. In the **Notifications** panel:

   1. If you need to apply an Amazon SNS topic from another region, choose **Add another Region**\.

1. For each **Region** in the Amazon Chime webhook or Slack channel, select the Amazon SNS topic you want to add\.

1. When finished, choose **Save**\.

1. To check for the subscription, click on any subscription entry in the AWS Chatbot console\. The Amazon SNS console opens, showing the list of subscriptions for the selected topic\.

### Step 3: Test notifications from AWS services to Amazon Chime or Slack<a name="test-notifications-slackt"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Amazon Chime or Slack chat room, you can test your setup by sending a notification\. To test your notifications, ensure your topics are assigned to a service supported by AWS Chatbot\. For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\. You can also test notifications by using CloudWatch\. For more information, see [Test notifications from AWS services to Amazon Chime or Slack using CloudWatch](test-notifications-cw.md)\.

**Testing notifications with configured clients**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client you want to test\.

1. In the configured client, choose the channel or webhook to send a test notification to\.

1. Choose **Send test message**\.

1. View the confirmation message at the top of the screen that shows a message was sent to your Amazon SNS topic\.

1. Confirm the test message in your Amazon Chime chat room or Slack channel\.

### Next steps<a name="next-steps-slackt"></a>

After you configure your chat clients and test that your notifications are working, you might want to explore some of the following topics:
+ Learn about which other AWS services you can integrate with AWS Chatbot in [Monitoring AWS services using AWS Chatbot](related-services.md)\.
+ Learn about using AWS CLI syntax on your Slack channels in [Running AWS CLI commands from chat channels](chatbot-cli-commands.md)\.