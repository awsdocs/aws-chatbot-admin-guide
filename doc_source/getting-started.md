--------

AWS Chatbot is in beta and is subject to change\.

--------

# Getting Started with AWS Chatbot<a name="getting-started"></a>

After you set up AWS Chatbot and its Amazon Simple Notification Service topic subscriptions, you can begin using these new resources to help manage your AWS infrastructure\. 

Most services supported by AWS Chatbot use Amazon CloudWatch to send alarms or event reports to SNS topics\. To quickly verify that AWS Chatbot is working with your Amazon SNS topics, [you can set up a CloudWatch alarm to send a test notification to AWS Chatbot](#Send-messages-to-chatbot)\.

If you need to customize an IAM role to work with AWS Chatbot, [you can use the procedure in this topic](#AWS::Chatbot::Role)\.

## Prerequisites<a name="getting-started-prerequisites"></a>

To ensure that your chat notifications work correctly, you need the following:
+ An Amazon SNS topic configured in a Slack channel or an Amazon Chime webhook in the AWS Chatbot\. To create a new SNS topic for testing, open the Amazon SNS console at [Simple Notification Service](https://console.aws.amazon.com/sns/)\. Subscribe the new topic to the AWS Chatbot Slack channel or Amazon Chime webhook before you use the SNS topic elsewhere in AWS\.
+ IAM permissions and experience with all of the AWS services that you expect to use with AWS Chatbot\.
+ Experience with Amazon CloudWatch and its use of Amazon SNS topics\.

**Note**  
For information about subscribing an SNS topic to AWS Chatbot, see [Setting Up AWS Chatbot with Slack](setting-up.md#Setting_up_Slack) or [Setting Up AWS Chatbot with Amazon Chime](setting-up.md#Setting_up_Chime)\.

## Testing Notifications from AWS Services to Amazon Chime or Slack Chat Rooms<a name="Send-messages-to-chatbot"></a>

To verify that an Amazon Simple Notification Service \(Amazon SNS\) topic sends notifications to your Amazon Chime or Slack chat room, you can test your setup by sending a notification\. Any SNS topic can send notifications to your chat rooms, but the topic must be assigned to a service supported by AWS Chatbot\. For a complete list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

**Note**  
CloudWatch alarms and events are separately configured and have different characteristics for use with AWS Chatbot\. 

The following procedure uses a CloudWatch alarm because most AWS services supported by AWS Chatbot send their event and alarm data to CloudWatch\. 

You configure CloudWatch alarms using performance metrics from the services that are active in your account\. When you associate CloudWatch alarms with an Amazon SNS topic that is mapped to AWS Chatbot, the Amazon SNS topic sends the CloudWatch alarm notifications to the chat rooms\. For more information, see [Using AWS Chatbot with Other AWS Services](related-services.md) and the [Troubleshooting](chatbot-troubleshooting.md) topic\.

**To test notifications to configured chat clients**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create alarm**\.

1. Select the correct AWS **Region** at the top right of the AWS console, that contains the Amazon SNS topic you need\. \(**Tip:** to make sure you have the right region for your SNS topics for testing alarms, you can check the AWS Chatbot configuration to see the regions for all configured SNS topics in each channel or webhook\.\)

1. Choose **Select metric**, and choose the **SNS** service namespace\. \(All CloudWatch alarms use service *metrics* to generate their notifications, and you need to select one for this example\.\)

   1. Choose **Topic metrics**\.

   1. Choose the check box for the SNS topic next to its **Topic Name** and **Metric Name**\. Any SNS topics that you configured with AWS Chatbot appear in this list\.

      *Important*: if you do not see your desired Amazon SNS topic in the SNS Topic list, make sure to select the correct AWS Region in the AWS console when you begin configuring the new CloudWatch alarm\.

   1. Choose **Select metric**\.

   The **Specify metric and conditions** page shows a graph and other information about the metric and statistic\.

1.  For **Conditions** \(the circumstances under which the CloudWatch alarm fires and an action takes place\), choose the following options:

   1. For **Threshold type**, choose **Static**\.

   1. For **Whenever *metric* is**, choose **Lower/Equal <=threshold**\. 

   1. For **than\.\.\.**, specify a threshold value of **1**\. This setting ensures you will trigger the test notification within one minute\.

   1. Under **Additional configuration**, do the following: 

      1. For **Datapoints to alarm**, select **1 out of 1**\.

      1. For **Missing data treatment**, select **Treat missing data as bad**\.

   1. Choose **Next**\.

1. Choose **Configure actions**\. Here, you set the *action* to create SNS notifications when the metric threshold is exceeded\.

    For **Notification**, choose the following options\.

   1. For **Whenever this alarm state is\.\.\.**, choose **In Alarm**\.

   1. For **Select an SNS topic**, choose **Select an existing SNS topic**\. 

   1. For **Send a notification to\.\.\.**, choose your SNS topic that has a subscription to AWS Chatbot\. If the SNS topic is subscribed in AWS Chatbot, the endpoint value for AWS Chatbot appears in the **Email \(endpoints\)** field\. 
**Note**  
If the endpoint value doesn't appear in the **Email \(endpoints\)** field, make sure that the SNS topic is set up correctly in the Slack channel or Amazon Chime webhook\. For more information, see [Setting Up AWS Chatbot with Slack](setting-up.md#Setting_up_Slack) or [Setting Up AWS Chatbot with Amazon Chime](setting-up.md#Setting_up_Chime)\. 

   1. Choose **Next**\.

1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then, choose **Next**\.

1.  For **Preview and create**, confirm that the information and conditions are correct, then choose **Create alarm**\.

When the alarm triggers for the first time, you should receive the first test notification in your chat room, confirming that AWS Chatbot is working correctly and receiving alarm notifications from Amazon CloudWatch\.

## Configuring an IAM Role for AWS Chatbot<a name="AWS::Chatbot::Role"></a>

You can create new IAM roles in the AWS Chatbot console, which provides a convenient way to deploy the AWS Chatbot service\. You associate these roles with your Slack channels or Amazon Chime webhooks\. The AWS Chatbot console does not allow editing of IAM roles, including any roles that you've already created in the AWS Chatbot console\. 

**Note**  
AWS requires that you use the IAM console to edit IAM roles\. If you create roles in the AWS Chatbot console, you need to use the IAM console to edit them\. This might happen, for example, when you are using the AWS Chatbot service and a new release comes out that supports new features\.

Use the IAM console to edit AWS Chatbot roles\. You can use the entire set of IAM console features to specify permissions for your AWS Chatbot users\.

**To edit roles**

1. [Open the AWS Chatbot console](https://us-east-2.console.aws.amazon.com/chatbot/home?region=us-east-2#/chat-clients)\.

1. Choose the Slack channel or Amazon Chime webhook, and choose the IAM role associated with the channel or webhook\. 

   The IAM console opens, automatically showing role configuration page, with the **Permissions** tab displaying the selected role\. 

1. Choose **Attach Policies**\.
**Note**  
You can attach AWS managed policies and customer managed policies\. AWS Chatbot roles support both types of IAM policies\.

1. Choose the policy you want by choosing its name\. You can use the **Search** box to search for the policy by its name or by a partial string of characters\. For example, all IAM policies associated with AWS Chatbot include the character string **Chatbot** as part of the policy name\. Following are the preconfigured customer managed policies available for AWS Chatbot:
   + **AWS\-Chatbot\-NotificationsOnly\-Policy**
   + **AWS\-Chatbot\-LambdaInvoke\-Policy**
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