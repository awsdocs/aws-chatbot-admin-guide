--------

AWS Chatbot is in beta and is subject to change\.

--------

# Getting Started with AWS Chatbot<a name="getting-started"></a>

After you set up AWS Chatbot and its Amazon Simple Notification Service topic subscriptions, you can begin using these new resources to help manage your AWS infrastructure\. Most services supported by AWS Chatbot use Amazon CloudWatch to send alarms to SNS topics\. 

To quickly verify that AWS Chatbot is working with your Amazon SNS topics, [you can set up a CloudWatch alarm to send a test notification to AWS Chatbot](#Send-messages-to-chatbot)\.

If you need to customize an IAM role to work with AWS Chatbot, [you can use the procedure in this section](#AWS::Chatbot::Role)\.

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

The following procedure uses Amazon CloudWatch because most AWS services supported by AWS Chatbot send their event and alarm data to CloudWatch\. It also uses a CloudWatch alarm\. 

You configure CloudWatch alarms using performance metrics from the services that are active in your account\. When you associate CloudWatch alarms with an Amazon SNS topic that is mapped to AWS Chatbot, the Amazon SNS topic sends the CloudWatch alarm notifications to the chat rooms\. For more information, see [Using AWS Chatbot with Other AWS Services](related-services.md) and the [Troubleshooting](chatbot-troubleshooting.md) topic\.

**To test notifications to configured chat clients**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create alarm**\.

1. Choose **Select metric**, and choose the **SNS** service namespace\. \(All CloudWatch alarms use service *metrics* to generate their notifications, and you need to select one for this example\.\)

   1. Choose **Topic metrics**\.

   1. Choose the check box for the SNS topic next to its **Topic Name** and **Metric Name**\. Any SNS topics that you configured with AWS Chatbot appear in this list\.

   1. Choose **Select metric**\.

      The **Specify metric and conditions** page shows a graph and other information about the metric and statistic\.

1.  For **Conditions** \(the circumstances under which the CloudWatch alarm fires and an action takes place\), choose the following options:

   1. For **Threshold type**, choose **Static**

   1. For **Whenever *metric* is**, choose **Lower/Equal <=threshold**\. 

   1. For **than\.\.\.**, specify a threshold value of **1**\. This setting ensures you will readily trigger the test notification\.

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

For testing, you can use a AWS Chatbot IAM role that you create when you configure the chat clients for AWS Chatbot\. If you want to use an existing IAM role from your AWS account to manage AWS Chatbot processes, you can edit the role in the IAM console\.

**To configure an IAM role for AWS Chatbot**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles**, and choose the IAM role that you want to edit\.

1. Choose **Attach Policies**\.

   1. In the **Search** box, enter **CloudWatchRead**\. The **AWS Policies** list shows the **CloudWatchReadOnlyAccess** policy\. This policy has full List and Read privileges for Amazon CloudWatch\.

   1. Choose the check box for the **CloudWatchReadOnlyAccess** policy, and choose **Attach Policy**\.

1. Ensure that the selected role has the **CloudWatchReadOnlyAccess** entry in its list of policies\.

1. Choose the **Trust relationships** tab\. 

   You must also edit the trust relationships information for the IAM role\. This requires a small edit to the JSON properties in the IAM role\.

   1. Choose **Edit trust relationship**\. You edit the JSON policy document on this page\. For example, the following is a basic Admin role for an AWS account\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam::123456789012:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
              "StringEquals": {
                "sts:ExternalId": "YourExternalIDHere"
              }
            } 
          } << add a comma here
        ]
      }
      ```

   1. Add a comma \(,\) after the curly bracket denoting the end of the last statement block\.

   1. Copy and paste the following text\.

      ```
          {
            "Effect": "Allow",
            "Principal": {
              "Service": "chatbot.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
          }
      ```

      The result looks like the following\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Sid": "",
            "Effect": "Allow",
            "Principal": {
              "AWS": "arn:aws:iam::123456784321:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {
              "StringEquals": {
                "sts:ExternalId": "YourExternalIDHere"
              }
            }
          },
          {
            "Effect": "Allow",
            "Principal": {
              "Service": "chatbot.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
          }
        ]
      }
      ```

1. Choose **Update Trust Policy**\. The editor performs JSON validation\. If you made a mistake, you will be notified and can fix it\. You won't be able to save flawed JSON code\.

**Note**  
The role's **Trusted entities** list ensures that the IAM role has the permissions to manage and configure AWS Chatbot and all CloudWatch information\. The permissions also support correctly forwarding and formatting metrics data as charts to chat rooms\.