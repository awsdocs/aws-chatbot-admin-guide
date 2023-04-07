# Tutorial: Getting started with Amazon Chime<a name="chime-setupt"></a>

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

If you need to customize an IAM role to work with AWS Chatbot, [you can use the procedure in this topic](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites-chimet)
+ [Step 1: Setting up AWS Chatbot with Amazon Chime](#chime-setst)
+ [Step 2: Subscribe an Amazon SNS topic to AWS Chatbot](#subscribe-sns-topict)
+ [Step 3: Test notifications from AWS services to Amazon Chime or Slack](#test-notificationst)
+ [Next steps](#next-stepst)

### Prerequisites<a name="getting-started-prerequisites-chimet"></a>

Before you get started, make sure you've completed the tasks in [Setting up AWS Chatbot](getting-started.md#setting-up)\. You will need to choose a permissions scheme in the following procedure\. This scheme determines the permissions your channel members will have and what AWS Chatbot can do on your behalf\. For more information about AWS Chatbot permissions, see [Understanding permissions](understanding-permissions.md)\.

### Step 1: Setting up AWS Chatbot with Amazon Chime<a name="chime-setst"></a>

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

   1. For **Role**, choose **Create a new role from template**\. If you want to use an existing role instead, choose it from the **IAM Role** list\. To use an existing IAM role, you might need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

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

### Step 2: Subscribe an Amazon SNS topic to AWS Chatbot<a name="subscribe-sns-topict"></a>

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

### Step 3: Test notifications from AWS services to Amazon Chime or Slack<a name="test-notificationst"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Amazon Chime or Slack chat room, you can test your setup by sending a notification\. To test your notifications, ensure your topics are assigned to a service supported by AWS Chatbot\. For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\. You can also test notifications by using CloudWatch\. For more information, see [Test notifications from AWS services to Amazon Chime or Slack using CloudWatch](test-notifications-cw.md)\.

**Testing notifications with configured clients**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client you want to test\.

1. In the configured client, choose the channel or webhook to send a test notification to\.

1. Choose **Send test message**\.

1. View the confirmation message at the top of the screen that shows a message was sent to your Amazon SNS topic\.

1. Confirm the test message in your Amazon Chime chat room or Slack channel\.

### Next steps<a name="next-stepst"></a>

After you configure your chat clients and test that your notifications are working, you might want to explore some of the following topics:
+ Learn about which other AWS services you can integrate with AWS Chatbot in [Monitoring AWS services using AWS Chatbot](related-services.md)\.
+ Learn about using AWS CLI syntax on your Slack channels in [Running AWS CLI commands from chat channels](chatbot-cli-commands.md)\.