--------

AWS Chatbot is in beta and is subject to change\.

--------

# Troubleshooting AWS Chatbot<a name="chatbot-troubleshooting"></a>

## Notifications Aren't Sent to Chat Rooms<a name="chatbot-troubleshooting"></a>

**Notifications are sent by the AWS service to the Amazon Simple Notification Service \(Amazon SNS\) topics added to AWS Chatbot, but the notifications are not appearing in the chat rooms\.** 

Possible causes: 
+ The notification's originating service is not supported by AWS Chatbot\. 

  For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md#related-services.title)\.
+ AWS Chatbot supports CloudWatch Events only for specific AWS services\. If you map Amazon SNS topics to AWS Chatbot that are associated with any other AWS service, their notifications will only go to email\. 

  Supported services for CloudWatch Events forwarding to chat rooms include AWS Config, Amazon GuardDuty, AWS Health, AWS Security Hub, AWS Systems Manager, and AWS CloudFormation\. For services in which CloudWatch Events are not directly supported by AWS Chatbot, you can create an Amazon CloudWatch alarm\. You configure CloudWatch alarms for metrics on the services that are active in your account, such as EC2 state changes\. By associating alarms with an Amazon SNS topic that is mapped to AWS Chatbot, the chatbot sends the Amazon CloudWatch alarm notifications to the chat rooms\.
+ The SNS topic doesn't have a subscription to AWS Chatbot\. 

  In the Amazon SNS console, check the **Subscriptions** tab on the **Topics** page to see if it has a subscription\. If it doesn't, open the AWS Chatbot console, open your authorized client, and look at the **Configured channels** or **Configured webhooks** list\. Add a new channel or webhook configuration, and add the SNS topic\. Without this configuration, event notifications cannot reach the chat rooms\. 
+ The CloudWatch alarm notifications don't show the graphs from the reporting metrics\.

  The role doesn’t have CloudWatchRead permissions\. 

  In the AWS Chatbot console, create a new role\. This role requires the Notifications permissions policy that is available from the AWS Chatbot console when you configure a new webhook or Slack channel\. You can also edit the IAM role to add the needed CloudWatchRead permissions\.