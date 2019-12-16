--------

AWS Chatbot is in beta and is subject to change\.

--------

# Troubleshooting AWS Chatbot<a name="chatbot-troubleshooting"></a>

## Notifications Aren't Sent to Chat Rooms<a name="chatbot-subcription-troubleshooting"></a>

**I configured my AWS service to send Notifications to the Amazon Simple Notification Service \(Amazon SNS\) topics mapped to AWS Chatbot, but the notifications aren't appearing in the chat rooms\.** 

Possible causes: 
+ **The notification's originating service is not supported by AWS Chatbot\.**

  For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md#related-services.title)\.
+ **AWS Chatbot supports CloudWatch Events only for specific AWS services\.**

  Services that support CloudWatch Events forwarding by Amazon SNS topics to Slack or Amazon Chime chat rooms include:
  + [AWS Billing and Cost Management](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/sns-alert-chime.html)
  + [AWS Config Events](https://docs.aws.amazon.com/config/latest/developerguide/monitor-config-with-cloudwatchevents.html)
  + [Amazon GuardDuty Events](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings_cloudwatch.html)
  + [AWS Health Events](https://docs.aws.amazon.com/health/latest/ug/cloudwatch-events-health.html)
  + [ AWS Security Hub Events](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cloudwatch-events.html#securityhub-cwe-send)
  + [AWS Systems Manager Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-cloudwatch-events.html) including the following:
    + AWS Systems Manager [Configuration Compliance Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-compliance-fixing.html)
    + AWS Systems Manager [Maintenance Windows Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-sns-mw-register.html)
    + AWS Systems Manager [Parameter Store Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-paramstore-cwe.html)

  If you map Amazon SNS topics that are associated with any other AWS service, their CloudWatch Events notifications will go only to email or other standard SNS targets, and will not work with AWS Chatbot\.
**Note**  
Some AWS services support Amazon CloudWatch alarms for reporting and monitoring\. You configure CloudWatch alarms using performance metrics from the services that are active in your account\. Amazon Elastic Compute Cloud \(EC2\), with its collection of metrics, is a good example\. When you associate CloudWatch alarms with an Amazon SNS topic that is mapped to AWS Chatbot, the Amazon SNS topic sends the CloudWatch alarm notifications to the chat rooms\.
+ **The SNS topic doesn't have a subscription to AWS Chatbot\.**

  In the Amazon SNS console, check the **Subscriptions** tab on the **Topics** page to see if the topic has a subscription\. If the topic doesn't, open the AWS Chatbot console, then open your authorized client, and look at the **Configured channels** or **Configured webhooks** list\. Add a new channel or webhook configuration, and add the SNS topic\. Without this configuration, event notifications can't reach the chat rooms\. 
+ **Your SNS topic subscription to the AWS Chatbot has the Enable raw message delivery setting enabled\.**

  Don't enable the **Enable raw message delivery** feature for any SNS topic subscriptions to AWS Chatbot\.

## Other AWS Chatbot Issues<a name="chatbot-notification-troubleshooting"></a>
+ **CloudWatch alarm notifications don't show the graphs from the reporting metrics\.**

  The IAM role doesn’t have CloudWatchRead permissions\. 

  In the AWS Chatbot console, create a new role\. This role requires the Notifications permissions policy from the AWS Chatbot console when you configure a new webhook or Slack channel\. You can also edit your IAM role to [add the CloudWatchRead permissions](getting-started.md#AWS::Chatbot::Role) for AWS Chatbot\.
+ **When I set up an SNS topic in the AWS Billing and Cost Management console to forward notifications to the AWS Chatbot, I get a "Please comply with SNS Topic ARN format" error message\.**

  If the AWS Billing and Cost Management console displays an error message for the SNS topic you want to use for notifications, [you can edit the SNS topic's permissions policy so it can forward Budget notifications\.](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-sns-policy.html) 

  Do this if you have already configured an SNS topic that has a subscription to AWS Chatbot or have configured a new SNS topic\. It is not needed if you want to use an Amazon SNS topic that is already configured and working with AWS Billing and Cost Management\. [You can then set up that topic with a subscription to AWS Chatbot](setting-up.md#Setup_intro)\.