# Tutorial: Get started with Microsoft Teams<a name="teams-setup"></a>

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

**Topics**
+ [Prerequisites](#getting-started-prerequisites-teams)
+ [Step 1: Setting up AWS Chatbot with Microsoft Teams](#teams-client-setup)
+ [Step 2: Test notifications from AWS services to Microsoft Teams](#test-notifications-teams)
+ [Next steps](#next-steps-teams)

## Prerequisites<a name="getting-started-prerequisites-teams"></a>

Before you get started, make sure you've completed the tasks in [Setting up AWS Chatbot](getting-started.md#setting-up)\. You should also ensure Microsoft Teams is installed and approved by your organization administrator\. You will need to choose a permissions scheme in the following procedure\. This scheme determines the permissions your channel members will have and what AWS Chatbot can do on your behalf\. For more information about AWS Chatbot permissions, see [Understanding permissions](understanding-permissions.md)\.

**Note**  
The following IAM permissions are required to create a Microsoft Teams configuration:  
GetMicrosoftTeamsOauthParameters
RedeemMicrosoftTeamsOauthCode
CreateMicrosoftTeamsChannelConfiguration
If you have less that administrative permissions, ensure you have the aforementioned permissions to create a configuration\.

## Step 1: Setting up AWS Chatbot with Microsoft Teams<a name="teams-client-setup"></a>

### <a name="teams-sets"></a>

To allow AWS Chatbot to send notifications or run commands in your Microsoft Teams channel, you must configure AWS Chatbot with Microsoft Teams\.

**To configure a Microsoft Teams client**

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Under **Configure a chat client**, choose **Microsoft Teams**, then choose **Configure client**\.

1. Copy and paste your Microsoft Teams channel URL\. Your channel URL contains your tenant, team, and channel IDs\. You can find your channel URL by right clicking on the channel in your Microsoft Teams channel list and copying the link\.

1. Choose **Configure**\.
**Note**  
After choosing **Configure**, you'll be redirected to Microsoft Team's authorization page to request permission for AWS Chatbot to access your information\.

1. On the Microsoft Teams authorization page, choose **Accept**\.

1. From the **Microsoft Teams** page, choose **Configure new channel**\. 

1. Under **Configuration details**, enter a name for your configuration\. The name must be unique across your account and can't be edited later\.

1. If you want to enable logging for this configuration, choose **Publish logs to Amazon CloudWatch Logs**\. For more information, see [Amazon CloudWatch Logs for AWS Chatbot](cloudwatch-logs.md)\.
**Note**  
There is an extra charge for using CloudWatch Logs\.

1. For **Team channel**, paste your Microsoft Teams channel URL\.

1. Choose your **Role Setting**\.
**Tip**  
 Your role setting dictates what permissions your channel members have\. A channel role gives all members the same permissions\. This is useful if your channel members typically perform the same actions in Microsoft Teams\. A user role requires your channel members to choose their own roles\. As such, different users in your channels can have different permissions\. This is useful if your channel members are diverse or you don’t want new channel members to perform actions as soon as they join the channel\. For more information, see [Role setting](understanding-permissions.md#role-settings)\. 

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
To run most CLI commands from your Microsoft Teams channel, ensure you select **All actions permitted**\.

1. Choose your notification settings:

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the Amazon SNS topic for the client subscription\. This topic determines the content that's sent to the Microsoft Teams channel\. If the region has additional SNS topics, you can choose them from the same dropdown list\.

   1. To add an Amazon SNS topic from another AWS Region to the notification subscription, choose **Add another Region**\. 
**Note**  
For a tutorial on subscribing existing Amazon SNS topics to AWS Chatbot, see [Tutorial: Subscribing an Amazon SNS topic to AWS Chatbot](subscribe-sns-topic.md)\.

1. Choose **Configure**\. 

**Note**  
You can configure a Microsoft Teams channel to run commands to your AWS account\. For more information, see [Running AWS CLI commands from chat channels](chatbot-cli-commands.md)\.

Notifications from supported services that publish to the chosen Amazon SNS topics will now appear in the Microsoft Teams channel\.

You can configure as many channels with as many topics as you need\.

The SNS topics you choose also must be configured in the services for which you want to receive notifications\. For more information, see [Monitoring AWS services using AWS Chatbot](related-services.md)\.

If you want to allow AWS Chatbot to answer questions about your AWS resources, turn on AWS Resource Explorer in the Resource Explorer Console\. For more information, see [Getting started with Resource Explorer ](https://docs.aws.amazon.com/resource-explorer/latest/userguide/getting-started.html)in the *AWS Resource Explorer User Guide*\.

## Step 2: Test notifications from AWS services to Microsoft Teams<a name="test-notifications-teams"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Microsoft Teams channel, you can test your setup by sending a notification\. To test your notifications, ensure your topics are assigned to a service supported by AWS Chatbot\. For a list of supported services, see [Monitoring AWS services using AWS Chatbot](related-services.md)\. You can also test notifications by using CloudWatch\. For more information, see [Test notifications from AWS services to Microsoft Teams using CloudWatch](test-notifications-cw.md)\.

**Testing notifications with configured clients**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client you want to test\.

1. In the configured client, choose the channel to send a test notification to\.

1. Choose **Send test message**\.

1. View the confirmation message at the top of the screen that shows a message was sent to your Amazon SNS topic\.

1. Confirm the test message in your Microsoft Teams channel\.

## Next steps<a name="next-steps-teams"></a>

After you configure your chat clients and test that your notifications are working, you might want to explore some of the following topics:
+ Learn about which other AWS services you can integrate with AWS Chatbot in [Monitoring AWS services using AWS Chatbot](related-services.md)\.
+ Learn about what actions you can perform using AWS Chatbot in [Performing actions](performing-actions.md)\.
+ Learn how to receive AWS CodeStar notifications in your channels in [Tutorial: Receive Developer Tools notifications in Microsoft Teams](teams-codestar.md#teams-codestar.title)\.