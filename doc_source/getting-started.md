# Getting started with AWS Chatbot<a name="getting-started"></a>


|  | 
| --- |
| AWS resource management using CLI commands in Slack is in public preview and is subject to change\. Your use of this feature is subject to the Betas and Previews terms of the [AWS Service Terms](https://aws.amazon.com/service-terms/) \(Section 2\)\. | 

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

If you need to customize an IAM role to work with AWS Chatbot, [you can use the procedure in this topic](#editing-iam-roles-for-chatbot)\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites)
+ [Step 1: Set up chat clients for AWS Chatbot](#chat-client-setup)
+ [Step 2: Subscribe an Amazon SNS topic to AWS Chatbot](#subscribe-sns-topic)
+ [Step 3: Test notifications from AWS services to Amazon Chime or Slack](#test-notifications)
+ [Test notifications from AWS services to Amazon Chime or Slack using CloudWatch](test-notifications-cw.md)
+ [Remove chat rooms](#removing-chatrooms)
+ [Configuring an IAM role for AWS Chatbot](#editing-iam-roles-for-chatbot)
+ [Next steps](#next-steps)

## Prerequisites<a name="getting-started-prerequisites"></a>

Before you get started, make sure you've completed the tasks in [Setting up AWS Chatbot](setting-up.md)\. You will need to choose a permissions scheme in the following procedure\. This scheme determines the permissions your channel members will have\.

## Step 1: Set up chat clients for AWS Chatbot<a name="chat-client-setup"></a>

You use the AWS Chatbot console to configure Amazon Chime and Slack clients to receive notifications from Amazon Simple Notification Service \(Amazon SNS\) topics\. 

**Note**  
When you configure your clients, don't enable the **Enable raw message delivery** feature for any Amazon SNS topic subscription that you want to use for AWS Chatbot\. 

AWS Chatbot requires an AWS Identity and Access Management \(IAM\) role with Amazon CloudWatch read permissions and a trust policy that allows AWS Chatbot to use those permissions on your behalf\. When you configure AWS Chatbot, you can create a role with a predefined set of policies to display CloudWatch charts in AWS Chatbot notifications\. 

You can also use an existing IAM role that you can configure for use with AWS Chatbot\. For more information, see [Configuring an IAM role for AWS Chatbot](#editing-iam-roles-for-chatbot)\. For simplicity, particularly in testing your setup, we recommend using the IAM role with predefined policies that you can configure in AWS Chatbot\. 

### Setting up AWS Chatbot with Slack<a name="slack-setup"></a>

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

1. Choose your **Role Setting**\. You can choose a channel IAM role or user roles\. A channel IAM role allows channel members to share the same permissions\. User roles require your channel members to choose their own roles\. If you choose to use a channel IAM role, your users can still choose to use their own user roles\. For more information about role setting, see [Role setting](security-iam.md#role-setting)\.

------
#### [ Channel IAM role ]

   1. For **Role setting**, choose **Channel IAM role**\.

   1. For **Channel IAM role**, choose **Create new role**\. If you want to use an existing role instead, choose **Use an existing role**\. To use an existing IAM role, you will need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](#editing-iam-roles-for-chatbot)\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9, \.\\w\+=,\.@\-\_\.

   1. For **Role policy template**, choose the template you wish to use\.

------
#### [ User roles ]

   1. For **Role setting**, choose **User roles**\.

------

1. Select the policies that will make up your [channel guardrails](security-iam.md#channel-guardrails)\. Your channel guardrails control what actions are available to your channel members\.
**Note**  
If you initially had permission to run Lambda invoke, it is contained in **All actions permitted**\.
**Note**  
To run most CLI commands from your Slack channel, ensure you select **All actions permitted**\.

1. Choose your notification settings:

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the Amazon SNS topic for the client subscription\. This topic determines the content that's sent to the Slack channel\. If the region has additional SNS topics, you can choose them from the same dropdown list\.

   1. To add an Amazon SNS topic from another AWS Region to the notification subscription, choose **Add another Region**\. 

1. Choose **Save**\. 

1. Set **User permissions**:
**Note**  
You can choose to enable a user role requirement\. This requires channel members to apply a user role before running commands in Slack\. For more information, see [User role requirement](security-iam.md#role-req)\.

   1. Under **Account settings**, choose **User permissions**\.

   1. In **User role requirement**, choose if you want to enable a user role requirement\.

1. Add AWS Chatbot to the Slack workspace:

   1. In Slack, on the left navigation pane, choose **Apps**\.
**Note**  
If you do not see **Apps** in the left navigation pane, choose **More**, then choose **Apps**\.

   1. If AWS Chatbot is not listed, choose the **Browse Apps Directory** button\.

   1. Browse the directory for the AWS Chatbot app and then choose **Add** to add AWS Chatbot to your workspace\.

Notifications from supported services that publish to the chosen Amazon SNS topics will now appear in the Slack channel\.

You can configure as many channels with as many topics as you need\.

**Note**  
If you configure a private Slack channel, run the `/invite @AWS` command in Slack to invite the AWS Chatbot to the chat room\.

The SNS topics you choose also must be configured in the services for which you want to receive notifications\. For more information, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

### Setting up AWS Chatbot with Amazon Chime<a name="chime-setup"></a>

To set up AWS Chatbot for Amazon Chime, get the webhook URL for your team's chat room from Amazon Chime\.

**Prerequisite**

You must be an Amazon Chime chat room admin and have the ability to manage webhooks\.

**To configure an Amazon Chime client**

1. [Open Amazon Chime](http://app.chime.aws/)\.

1. For **Amazon Chime**, choose the chat room that you want to set up to receive notifications through AWS Chatbot\.

1. Choose the Room settings icon on the top right and choose **Manage Webhooks and Bots**\.

   Amazon Chime displays the webhooks associated with the chat room\.
**Note**  
You can have multiple webhooks in a single Amazon Chime chat room\.   
For example, in an **Amazon Chime** chat room, one webhook could send notifications for Amazon CloudWatch alarms and another webhook could send AWS Security Hub security alerts\. Each webhook receives notifications only for the SNS topics subscribed to it\. All chat room members can see all of the notifications from each of the SNS topics\. 

1. For the webhook, choose **Copy URL** and choose **Done**\.

   If you need to create a new webhook for the chat room, choose **Add webhook**, enter a name for the webhook in the **Name** field, and choose **Create**\.

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Choose **Configure new client**\.

1. Choose **Amazon Chime** and choose **Configure**\.

1. Under **Configuration details**, enter a name for your configuration\. The name must be unique across your account and can't be edited later\.

1. If you want to enable logging for this configuration, choose **Send logs to CloudWatch**\. For more information, see [Amazon CloudWatch Logs for AWS Chatbot](cloudwatch-logs.md)\.
**Note**  
There is an extra charge for using CloudWatch Logs\.

1. For **Configure Amazon Chime webhook**, do the following\.

   1. Paste the webhook URL that you copied from Amazon Chime\.

   1. For **Webhook description**, use the following naming convention to describe the purpose of the webhook: **Chat\_room\_name/Webhook\_name**\. This helps you associate Amazon Chime webhooks with their AWS Chatbot configurations\.

1. For **IAM permissions**, set the IAM permissions for AWS Chatbot\.

   1. For **Role**, choose **Create a new role from template**\. If you want to use an existing role instead, choose it from the **IAM Role** list\. To use an existing IAM role, you might need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](#editing-iam-roles-for-chatbot)\.

   1. For **Policy templates**, choose **Notification permissions**\. This is the IAM policy provided by AWS Chatbot\. It provides the necessary Read and List permissions for CloudWatch alarms, events and logs, and for Amazon SNS topics\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9\.

1. Set up the SNS topics that will send notifications to the Amazon Chime webhook\.

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the SNS topic for the client subscription\. This topic determines the content that's sent to the Amazon Chime webhook\. If the region has additional SNS topics, you can choose them from the same dropdown list\.

   1. If you want to add an SNS topic from another Region to the notification subscription, choose **Add another Region**\. 

1. Choose **Configure**\. 

Notifications from supported services that publish to the chosen SNS topics will now appear in the Amazon Chime chat room\.

You can configure as many webhooks as you need\. The SNS topics that you choose also must be configured in the services for which you want to receive notifications\. For more information, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

**Note**  
You can configure a Slack channel to run commands to your AWS account\. For more information, see [Running AWS CLI Commands from Slack Channels](chatbot-cli-commands.md)\.

## Step 2: Subscribe an Amazon SNS topic to AWS Chatbot<a name="subscribe-sns-topic"></a>

You can quickly subscribe existing Amazon SNS topics to the AWS Chatbot service\. You associate the new subscriptions to a Slack channel or Amazon Chime webhook\. After doing so, the messages from those topics will appear in the Slack or Amazon Chime chat rooms\. The Amazon SNS topics must be associated with AWS services that AWS Chatbot supports, and may also require further configuration, such as association with a CloudWatch rule\. This procedure is most useful if you have SNS topics that are already doing significant work with CloudWatch Events and CloudWatch alarms in AWS cloud services supported by AWS Chatbot\.

**Note**  
You can set up each supported AWS service to *target* one or more SNS topics to send notifications to AWS Chatbot\. You do this using each AWS service's console, or using AWS CloudFormation\. If you already have Amazon SNS topics set as targets for supported services, you can configure AWS Chatbot to use those topics\. Notifications from subscribed topics will automatically appear in your Slack or Amazon Chime clients without further configuration\. 

**Note**  
If your SNS topic is encrypted, you must add a section to your AWS KMS key policy to give the sending service permissions to post events to the encrypted SNS topics\. For more information, see [Setting up Amazon SNS topics](setting-up.md#chatbot-sns)\.

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Under **Configured clients**, choose Slack or Amazon Chime\. 

1. Choose any channel in the Slack workspace configuration or webhook in the Amazon Chime webhooks list\.

1. Choose **Edit**\. The configuration page for the channel or webhook appears\. Note that the **Region** Notifications is already configured\.

1. In the **Notifications** panel:

   1. If you need to apply an Amazon SNS topic from another region, choose **Add another Region**\.

1. For each **Region** in the Amazon Chime webhook or Slack channel, select the Amazon SNS topic you want to add\.

1. When finished, choose **Save**\.

1. To check for the subscription, click on any subscription entry in the AWS Chatbot console\. The Amazon SNS console opens, showing the list of subscriptions for the selected topic\.

## Step 3: Test notifications from AWS services to Amazon Chime or Slack<a name="test-notifications"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Amazon Chime or Slack chat room, you can test your setup by sending a notification\. To test your notifications, ensure your topics are assigned to a service supported by AWS Chatbot\. For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\. You can also test notifications by using CloudWatch\. For more information, see [Test notifications from AWS services to Amazon Chime or Slack using CloudWatch](test-notifications-cw.md)\.

**Testing notifications with configured clients**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client you want to test\.

1. In the configured client, choose the channel or webhook to send a test notification to\.

1. Choose **Send test message**\.

1. View the confirmation message at the top of the screen that shows a message was sent to your Amazon SNS topic\.

1. Confirm the test message in your Amazon Chime chat room or Slack channel\.

## Remove chat rooms<a name="removing-chatrooms"></a>

### Removing an authorized Slack client from AWS Chatbot<a name="removing-slack-client"></a>

When necessary, you can remove a Slack chat client from the AWS Chatbot configuration\. Doing so *deauthorizes* the Slack client, which revokes the permissions that AWS Chatbot uses to operate with Slack\. 

Before deauthorizing a Slack client, you must delete all Slack channels\. Deleting the channels first prevents accidentally deleting the Slack workspace\. 

**To remove a Slack client**

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1.  Choose **Configured clients**\.

1. On the **Configured clients** page, choose the Slack client\. 

1. Choose each channel in the Slack workspace configuration and choose **Delete**\.

1. After you finish deleting all Slack channels from the workspace, choose** Remove workspace configuration**\. AWS deletes the Slack workspace\. 

### Removing an Amazon Chime webhook from AWS Chatbot<a name="removing-chime-room"></a>

You can remove an Amazon Chime webhook from the AWS Chatbot configuration\. Doing so *deauthorizes* the Amazon Chime webhook, which revokes the permissions that AWS Chatbot uses to operate with Amazon Chime\. 

**To remove an Amazon Chime webhook**

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1.  Choose **Configured clients**\.

1. On the **Configured clients** page, choose the Amazon Chime webhook you want to delete\.

1. Choose **Delete**\.

## Configuring an IAM role for AWS Chatbot<a name="editing-iam-roles-for-chatbot"></a>

You can create new IAM roles in the AWS Chatbot console, which provides a convenient way to deploy the AWS Chatbot service\. You associate these roles with your Slack channels or Amazon Chime webhooks\. The AWS Chatbot console does not allow editing of IAM roles, including any roles that you've already created in the AWS Chatbot console\. 

**Note**  
AWS requires that you use the IAM console to edit IAM roles\. If you create roles in the AWS Chatbot console, you need to use the IAM console to edit them\. This might happen, for example, when you are using the AWS Chatbot service and a new release comes out that supports new features\.

Use the IAM console to edit AWS Chatbot roles\. You can use the entire set of IAM console features to specify permissions for your AWS Chatbot users\.

**Note**  
All users in the Slack channel or Amazon Chime chat room will have the permissions defined by the role\.

**To edit roles**

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Choose the Slack channel or Amazon Chime webhook, and choose the IAM role associated with the channel or webhook\. 

   The IAM console opens, automatically showing role configuration page, with the **Permissions** tab displaying the selected role\. 

1. Choose **Attach Policies**\.
**Note**  
You can attach AWS managed policies and customer managed policies\. AWS Chatbot roles support both types of IAM policies\.

1. Choose the policy you want by choosing its name\. You can use the **Search** box to search for the policy by its name or by a partial string of characters\. For example, all IAM policies associated with AWS Chatbot include the character string **Chatbot** as part of the policy name\. Following are the preconfigured customer managed policies available for AWS Chatbot:
   + **AWS\-Chatbot\-NotificationsOnly\-Policy**
   + **AWS\-Chatbot\-ReadOnly\-Commands\-Policy**

   You can use these policies to create your own policies that are less permissive and specify the resources their users can access in their roles\. You can also substitute these custom policies for the ones listed here\.

1. You can also attach any of three AWS managed policies to any role\. You can use these policies as templates to create your own policies\.
   + **ReadOnlyAccess**
   + **CloudWatchReadOnlyAccess**
   + **AWSSupportAccess**

   The **ReadOnlyAccess** policy is automatically attached to any role you create in the AWS Chatbot console\. 

   The **AWSSupportAccess** policy is the only AWS managed policy that appears in the AWS Chatbot console when you configure new roles there\.

   You can use these policies to create your own policies that are less permissive and specify the resources their users can access\. You can substitute these custom policies for the ones listed here\.

1. Choose each of the policies you want to attach to the role and choose **Attach policy**\. If needed, use the Search box to locate the policies you're looking for\.

   After you click **Attach policy**, the role's **Permissions** page opens and shows the change in the **Permissions** list\.

**Note**  
For more information about the customer managed policies and AWS managed policies described in this section, see [IAM Policies for AWS Chatbot](chatbot-iam-policies.md)\.  
For more information about editing IAM policies, see [Editing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-edit.html)\. Exercise caution at all times when editing policies, and don't overwrite existing customer managed policies unless necessary\.

## Next steps<a name="next-steps"></a>

After you configure your chat clients and test that your notifications are working, you might want to explore some of the following topics:
+ Learn about which other AWS services you can integrate with AWS Chatbot in [Using AWS Chatbot with other AWS services](related-services.md)\.
+ Learn about using AWS CLI syntax on your Slack channels in [Running AWS CLI commands from Slack channels](chatbot-cli-commands.md)\.