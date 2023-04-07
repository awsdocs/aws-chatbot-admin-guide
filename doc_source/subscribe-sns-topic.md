# Tutorial: Subscribing an Amazon SNS topic to AWS Chatbot<a name="subscribe-sns-topic"></a>

You can quickly subscribe existing Amazon SNS topics to the AWS Chatbot service\. You associate the new subscriptions to a chat channel\. After doing so, the messages from those topics will appear in the channel\. The Amazon SNS topics must be associated with AWS services that AWS Chatbot supports, and may also require further configuration, such as association with a CloudWatch rule\. This procedure is most useful if you have SNS topics that are already doing significant work with CloudWatch Events and CloudWatch alarms in AWS cloud services supported by AWS Chatbot\.

**Note**  
You can set up each supported AWS service to *target* one or more SNS topics to send notifications to AWS Chatbot\. You do this using each AWS service's console, or using AWS CloudFormation\. If you already have Amazon SNS topics set as targets for supported services, you can configure AWS Chatbot to use those topics\. Notifications from subscribed topics will automatically appear in your chat channels without further configuration\. 

**Note**  
If your SNS topic is encrypted, you must add a section to your AWS KMS key policy to give the sending service permissions to post events to the encrypted SNS topics\. For more information, see [Setting up Amazon SNS topics](getting-started.md#chatbot-sns)\.

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Under **Configured clients**, choose your chat client\. 

1. Choose any channel in your chat client configuration\.

1. Choose **Edit**\. The configuration page for the channel appears\. Note that the **Region** Notifications is already configured\.

1. In the **Notifications** panel:

   1. If you need to apply an Amazon SNS topic from another region, choose **Add another Region**\.

1. For each **Region** in the chat channel, select the Amazon SNS topic you want to add\.

1. When finished, choose **Save**\.

1. To check for the subscription, click on any subscription entry in the AWS Chatbot console\. The Amazon SNS console opens, showing the list of subscriptions for the selected topic\.