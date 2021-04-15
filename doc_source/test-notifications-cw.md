# Test notifications from AWS services to Amazon Chime or Slack using CloudWatch<a name="test-notifications-cw"></a>

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
If the endpoint value doesn't appear in the **Email \(endpoints\)** field, make sure that the SNS topic is set up correctly in the Slack channel or Amazon Chime webhook\. For more information, see [Setting Up AWS Chatbot with Slack](getting-started.md#slack-setup) or [Setting Up AWS Chatbot with Amazon Chime](getting-started.md#chime-setup)\. 

   1. Choose **Next**\.

1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then, choose **Next**\.

1.  For **Preview and create**, confirm that the information and conditions are correct, then choose **Create alarm**\.

When the alarm triggers for the first time, you should receive the first test notification in your chat room, confirming that AWS Chatbot is working correctly and receiving alarm notifications from Amazon CloudWatch\.