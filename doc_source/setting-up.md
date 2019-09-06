--------

AWS Chatbot is in beta and is subject to change\.

--------

# Setting Up AWS Chatbot<a name="setting-up"></a>

To use AWS Chatbot, you set up Slack or Amazon Chime as an AWS Chatbot client, and then configure AWS Chatbot to use an Amazon Simple Notification Service \(Amazon SNS\) topic to deliver notifications to the chat rooms\. \. 

**Note**  
You can set up each supported AWS service to *target* one or more SNS topics to send notifications to AWS Chatbot\. You do this using each AWS service's console\. If you already have SNS topics set as targets for supported services, you can configure AWS Chatbot to use those SNS topics\. Notifications from subscribed topics will automatically appear in your Slack or Amazon Chime clients without further configuration\. 

If you don't already have an AWS account and an AWS Identity and Access Management \(IAM\) user, you need to create them first\.

**Topics**
+ [Sign Up for AWS](#setting-up-aws-sign-up)
+ [Create an IAM User](#setting-up-create-iam-user)
+ [Setting Up Chat Clients for AWS Chatbot](#Setup_intro)

## Sign Up for AWS<a name="setting-up-aws-sign-up"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Create an IAM User<a name="setting-up-create-iam-user"></a>

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Use your AWS account email address and password to sign in as the *[AWS account root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)* to the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add user**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \-job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

## Setting Up Chat Clients for AWS Chatbot<a name="Setup_intro"></a>

**Note**  
Don't enable the **Enable raw message delivery** feature for any Amazon SNS topic subscription that you want to use for AWS Chatbot\. 

You use the AWS Chatbot console to configure Amazon Chime and Slack clients to receive notifications from Amazon Simple Notification Service \(Amazon SNS\) topics\. 

AWS Chatbot requires an AWS Identity and Access Management \(IAM\) role with Amazon CloudWatch read permissions and a trust policy that allows AWS Chatbot to use those permissions on your behalf\. When you configure AWS Chatbot, you can use a provided IAM role, with built\-in permissions and a policy template, to display CloudWatch charts in AWS Chatbot notifications\. 

You can also use an existing IAM role that you can configure for use with AWS Chatbot\. For more information, see [Configuring the AWS Chatbot IAM Role](getting-started.md#AWS::Chatbot::Role)\. For simplicity, particularly in testing your setup, we recommend using the provided IAM role\. 

### Setting Up AWS Chatbot with Slack<a name="Setting_up_Slack"></a>

To allow AWS Chatbot to send notifications to your Slack channel, you must configure AWS Chatbot with Slack\. Owners of Slack workspaces can approve the use of the AWS Chatbot, and any workspace user can configure the workspace to receive notifications\.

**To configure a Slack client**

1. Log in to the AWS Chatbot console\.

1. Choose **Slack** and **Configure new client**\.

1. From the dropdown at the top right, choose the Slack workspace that you want to use with AWS Chatbot\.

   There's no limit to the number of workspaces that you can set up for AWS Chatbot, but you can set up only one at a time\.

1. Choose **Install**\. 

1. For **Slack channel**, choose the channel that you want to use\.
**Note**  
You can use private Slack channels with AWS Chatbot\. To do so, choose **Private channel**\. In Slack, copy the Channel ID of the private channel\. In AWS Chatbot, paste the ID into the **Channel URL** field\. \(If you copy the URL of the private Slack channel, the AWS Chatbot console shows only the Channel ID value when you paste it into the field\.\) In Slack, go to the private channel and enter the `/invite @AWS` command as a message in the chat window\. Doing so invites the AWS Chatbot service into the private channel \(without any other AWS services\)\. 

1. Define the **IAM permissions** that the chatbot uses for messaging your Slack chat room:

   1. For **Role**, choose **Create a new role from template**\.

   1. For **Policy templates**, choose **Notification permissions**\. This is the IAM policy template for AWS Chatbot\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9, \.\\w\+=,\.@\-\_\.

1. Choose the SNS topics that will send notifications to the Slack channel:

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the SNS topic for the client subscription\. This topic determines the content that will be sent to the Slack channel\. If more than one topic is available, you can choose them from the same dropdown\.

   1. To add an SNS topic from another AWS Region to the notification subscription, choose **Add another Region**\. 

1. Choose **Configure**\. 

Notifications from supported services that publish to the chosen SNS topics will now appear in the Slack channel\.

You can configure as many channels, with as many topics, as you need\.

**Note**  
If you configure a private Slack channel, run the `/invite @AWS` command in Slack to invite the AWS Chatbot to the chat room\.

### Setting Up AWS Chatbot with Amazon Chime<a name="Setting_up_Chime"></a>

To set up AWS Chatbot for Amazon Chime, get the webhook URL for your team's chat room from Amazon Chime\.

**Prerequisite**

You must be an Amazon Chime chat room admin and have the ability to manage webhooks\.

**To configure an Amazon Chime client**

1. [Open Amazon Chime](http://app.chime.aws/)\.

1. For **Amazon Chime**, choose the chat room that you want to set up to receive notifications through AWS Chatbot\.

1. Choose the Room settings icon  on the top right and choose **Manage Webhooks and Bots**\.

   Amazon Chime displays the webhooks associated with the chat room\.
**Note**  
You can have multiple webhooks in a single Amazon Chime chat room\.   
For example, in an **Amazon Chime** chat room, one webhook might send notifications for Amazon CloudWatch alarms and another webhook sends Security Hub security alerts\. Each webhook receives notifications only for the SNS topics subscribed to it\. All chat room members can see all of the notifications from each of the SNS topics\. 

1. For the webhook, choose **Copy URL** and choose **Done**\.

   If you need to create a new webhook for the chat room, choose **Add webhook**, enter a name for the webhook in the **Name** field, and choose **Create**\.

1. Open the [AWS Chatbot console](https://us-east-2.console.aws.amazon.com/chatbot/home?region=us-east-2#/chat-clients)\.

1. Choose **Configure new client**\.

1. Choose **Amazon Chime** and choose **Configure**\.

1. For **Configure Amazon Chime webhook**, do the following\.

   1. Paste the webhook URL that you copied from Amazon Chime\.

   1. For **Webhook description**, use the following naming convention to describe the purpose of the webhook: **Chat\_room\_name/Webhook\_name**\. This helps you associate Amazon Chime webhooks with their AWS Chatbot configurations\.

1. For **IAM permissions**, set the IAM permissions for AWS Chatbot\.

   1. For **Role**, choose **Create a new role from template**\. If you want to use an existing role, choose it from the **IAM Role** list\. To use an existing IAM role, you might need to modify it for use with AWS Chatbot\. For more information, see [Configuring an IAM Role for AWS Chatbot](getting-started.md#AWS::Chatbot::Role)\.

   1. For **Policy templates**, choose **Notification permissions**\. This is the IAM policy provided by AWS Chatbot\.

   1. For **Role name**, enter a name\. Valid characters: a\-z, A\-Z, 0\-9\.

1. Set up the SNS topics that will send notifications to the Amazon Chime webhook\.

   1. For **SNS Region**, choose the AWS Region that hosts the SNS topics for this AWS Chatbot subscription\.

   1. For **SNS topic**, choose the SNS topic for the client subscription\. This topic determines the content that will be sent to the Slack channel\. If more than one topic is available, you can choose them from the same dropdown\.

   1. If you want to add an SNS topic from another Region to the notification subscription, choose **Add another Region**\. 

1. Choose **Configure**\. 

The Amazon Chime webhook configuration is complete\. Notifications from supported services that publish to the chosen SNS topics will now appear in the Amazon Chime chat room\.

You can configure as many webhooks as you need\.

### Removing an Authorized Slack Client from the AWS Chatbot<a name="Removing_a_Slack_client"></a>

When necessary, you can remove a Slack chat client from the AWS Chatbot configuration\. Doing so *deauthorizes* the Slack client, which revokes the permissions that AWS Chatbot uses to operate with Slack\. 

Before deauthorizing a Slack client, you must delete all Slack channels\. Deleting the channels first prevents accidentally deleting the Slack workspace\. 

**To configure an Amazon Chime client**

1. Open the AWS Chatbot console and select **Configured clients**\.

1. In the **Configure clients** page, choose the Slack client\. 

1. Choose each channel in the Slack workspace configuration and choose **Delete channel**\.

1. After you finish deleting all Slack channels from the workspace, choose** Remove workspace configuration**\. AWS deletes the Slack workspace\. 