# Tutorial: Get started with Amazon Chime<a name="chime-setup"></a>

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites-chime)
+ [Step 1: Setting up AWS Chatbot with Amazon Chime](#chime-sets)
+ [Step 2: Test notifications from AWS services to Amazon Chime](#test-notifications)
+ [Next steps](#next-steps)

## Prerequisites<a name="getting-started-prerequisites-chime"></a>

Before you get started, make sure you've completed the tasks in [Setting up AWS Chatbot](getting-started.md#setting-up)\. You will need to choose a permissions scheme in the following procedure\. This scheme determines the permissions your channel members will have and what AWS Chatbot can do on your behalf\. For more information about AWS Chatbot permissions, see [Understanding permissions](understanding-permissions.md)\.

## Step 1: Setting up AWS Chatbot with Amazon Chime<a name="chime-sets"></a>

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
**Note**  
For a tutorial on subscribing existing Amazon SNS topics to AWS Chatbot, see [Tutorial: Subscribing an Amazon SNS topic to AWS Chatbot](subscribe-sns-topic.md)\.

1. Choose **Configure**\. 

Notifications from supported services that publish to the chosen SNS topics will now appear in the Amazon Chime chat room\.

You can configure as many webhooks as you need\. The SNS topics that you choose also must be configured in the services for which you want to receive notifications\. For more information, see [Monitoring AWS services using AWS Chatbot](related-services.md)\.

If you want to allow AWS Chatbot to answer questions about your AWS resources, turn on AWS Resource Explorer in the Resource Explorer Console\. For more information, see [Getting started with Resource Explorer ](https://docs.aws.amazon.com/resource-explorer/latest/userguide/getting-started.html)in the *AWS Resource Explorer User Guide*\.

## Step 2: Test notifications from AWS services to Amazon Chime<a name="test-notifications"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Amazon Chime chat room, you can test your setup by sending a notification\. To test your notifications, ensure your topics are assigned to a service supported by AWS Chatbot\. For a list of supported services, see [Monitoring AWS services using AWS Chatbot](related-services.md)\. You can also test notifications by using CloudWatch\. For more information, see [Test notifications from AWS services to Amazon Chime using CloudWatch](test-notifications-cw.md)\.

**Testing notifications with configured clients**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client you want to test\.

1. In the configured client, choose the webhook to send a test notification to\.

1. Choose **Send test message**\.

1. View the confirmation message at the top of the screen that shows a message was sent to your Amazon SNS topic\.

1. Confirm the test message in your Amazon Chime chat room\.

## Next steps<a name="next-steps"></a>

After you configure your chat clients and test that your notifications are working, you might want to explore some of the following topics:
+ Learn about which other AWS services you can integrate with AWS Chatbot in [Monitoring AWS services using AWS Chatbot](related-services.md)\.