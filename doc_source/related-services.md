--------

AWS Chatbot is in beta and is subject to change\.

--------

# Using AWS Chatbot with Other AWS Services<a name="related-services"></a>

AWS Chatbot works with a number of AWS services, including [Amazon CloudWatch](https://console.aws.amazon.com/cloudwatch/), [AWS Security Hub](https://console.aws.amazon.com/securityhub/), and [Amazon GuardDuty](https://console.aws.amazon.com/guardduty/)\. All services that work with AWS Chatbot use Amazon SNS topics as targets to send event and alarms notifications\. You may already have established Amazon SNS topics that send notifications to DevOps and development personnel as e\-mails\. Because AWS Chatbot redirects those Amazon SNS topics' notifications to chat rooms, you can map those already\-used Amazon SNS topics to an Slack channel or Amazon Chime webhook in the AWS Chatbot console\. 

If you are creating a new Amazon SNS topic, your services will require additional configuration\. 

**Topics**
+ [AWS Billing and Cost Management \(Through SNS topics\)](#aws-billing)
+ [AWS CloudFormation \(Stack Options\)](#cloud-formation)
+ [Amazon CloudWatch Alarms Support](#cloudwatch)
+ [Amazon CloudWatch Events Support](#cloudwatchevents)

You can set up the following AWS services to forward notifications to Amazon Chime or Slack chat rooms\.

## AWS Billing and Cost Management \(Through SNS topics\)<a name="aws-billing"></a>

AWS Billing and Cost Management helps AWS account holders plan service usage, service costs, and instance reservations\. You do this using several specific types of budgets, which track your unblended costs, subscriptions, refunds, and Reserved Instances\. The service sends AWS Budget Alerts to an Amazon SNS topic\. You then map the Amazon SNS topic in AWS Chatbot to send those notifications to your chat rooms\.

For information about setting up Amazon SNS topics for AWS budgets, see [Creating an Amazon SNS Topic for Budget Notifications](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-sns-policy.html) in the *AWS Billing and Cost Management User Guide*\.

## AWS CloudFormation \(Stack Options\)<a name="cloud-formation"></a>

AWS CloudFormation is an infrastructure management service that helps you model and set up Amazon Web Services resources so you can spend less time managing those resources and more time focusing on the applications that you run in AWS\. You create a template that describes all of the AWS resources that you want \(like Amazon EC2 instances or Amazon RDS DB instances\), and AWS CloudFormation provisions and configures those resources for you\. 

AWS Chatbot supports AWS CloudFormation notifications through Amazon SNS topics\. You enable support for AWS Chatbot\-enabled SNS topics by selecting them in each AWS CloudFormation stack configuration\. For more information, see [Setting AWS CloudFormation Stack Options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide//cfn-console-add-tags.html) in the *AWS CloudFormation User Guide*\. 

## Amazon CloudWatch Alarms Support<a name="cloudwatch"></a>

To monitor performance and operating metrics for AWS services, and send notifications when thresholds are breached, you can create alarms in CloudWatch\. CloudWatch sends an Amazon SNS message or performs an action when the alarm changes state\. The `Alarms` action can send a notification to an Amazon SNS topic that forwards the notification to AWS Chatbot for viewing in chat rooms\.

Any metric, for any AWS service, that CloudWatch alarm actions can report can also be shared by an Amazon SNS topic through the AWS Chatbot to chat rooms\. This includes alarms for services such as EC2\. 

For information about setting up Amazon SNS topics to forward CloudWatch alarms, see [Set Up Amazon SNS Notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring//US_SetupSNS.html) in the *Amazon CloudWatch User Guide*\.

Because CloudWatch alarms use Amazon SNS topics to forward alarm notifications, you only need to map the associated Amazon SNS topic to your Slack channel or Amazon Chime webhook configuration in AWS Chatbot\.

AWS Chatbot also supports several AWS services through CloudWatch Events\. For more information, see the following section\.

## Amazon CloudWatch Events Support<a name="cloudwatchevents"></a>

AWS Chatbot supports several AWS services through [https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\. CloudWatch uses CloudWatch Events rules to help manage AWS service events and how you respond to them\. You can use these rules to associate an SNS topic \(or other actions\) with an event type from any AWS service\.

You map the SNS topic to the CloudWatch Events rule, and then map it to an Slack channel or Amazon Chime webhook in the AWS Chatbot console\. When a service event matches the rule, the rule's SNS topic sends a notification to the chat room\. 

AWS Chatbot supports CloudWatch Events for the following AWS services: Amazon EventBridge, AWS Config, Amazon GuardDuty, AWS Health, AWS Security Hub, and AWS Systems Manager\.

### [Amazon EventBridge](https://console.aws.amazon.com/events/)<a name="eventbridge"></a>

The Amazon EventBridge service provides enterprises with real time access to resource changes in AWS, Software\-as\-a\-Service \(SaaS\) applications, and custom applications without writing code\. You can use EventBridge as a complete functional substitute for CloudWatch Events\. CloudWatch Events users can use the EventBridge console to access and edit their [existing event bus configurations](https://docs.aws.amazon.com/lumberyard/latest/userguide/ebus-in-depth.html), CloudWatch Events rules, and existing events, and configure SNS topics to forward event notifications, without using the CloudWatch console\. 

EventBridge events have a close relationship to CloudWatch Events\. EventBridge uses the same CloudWatch Events API, so your existing CloudWatch Events and API usage remain the same\. You can also configure your own event rules directly in the EventBridge console without ever using the CloudWatch console\. 

AWS Chatbot fully supports operation with EventBridge\. You can configure any SNS topic in EventBridge and map the same topic in the AWS Chatbot console to send associated events to your chat rooms\. For more information about creating event rules in EventBridge, see [Creating an EventBridge Rule That Triggers on an Event from an AWS Resource](https://docs.aws.amazon.com/eventbridge/latest/userguide//create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\. You choose the appropriate SNS topic with the **Select targets** setting\.

### [AWS Config](https://console.aws.amazon.com/config/)<a name="aws-config"></a>

AWS Config performs resource oversight and tracking for auditing and compliance, config change management, troubleshooting, and security analysis\. It provides a detailed view of AWS resources configuration in your AWS account\. The service also shows how resources relate to one another and how they were configured in the past, so you can see how configurations and relationships change over time\. 

For AWS Config monitoring, [you configure Amazon CloudWatch Events rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) or [EventBridge rules](https://docs.aws.amazon.com/eventbridge/latest/userguide//create-eventbridge-rule.html) to forward AWS Config events notifications to an Amazon SNS topic\. You can then map that topic to AWS Chatbot to track those event notifications in chat rooms\.

 For more information, see [ Notifications for AWS Config](https://docs.aws.amazon.com/config/latest/developerguide//notifications-for-AWS-Config.html) in the *AWS Config Developer's Guide*\.

### [Amazon GuardDuty](https://console.aws.amazon.com/guardduty/)<a name="aws-guardduty"></a>

Amazon GuardDuty is a security threat monitoring service that detects and reports on potential security threats in your AWS account\. It uses threat intelligence feeds, such as lists of malicious IPs and domains, and machine learning to identify possible unauthorized and malicious activity in your AWS environment\.

GuardDuty reports its security incidents and threats through *findings*\. Findings appear in the GuardDuty console and automatically appear as CloudWatch Events\. [You then create Amazon CloudWatch Events rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) or [EventBridge rules](https://docs.aws.amazon.com/eventbridge/latest/userguide//create-eventbridge-rule.html), so these events appear as notifications to a selected Amazon SNS topic\. You then map that SNS topic to a Slack channel or Amazon Chime webhook in AWS Chatbot\.

 For more information, see [Monitoring Amazon GuardDuty Findings with Amazon CloudWatch Events](https://docs.aws.amazon.com/guardduty/latest/ug//guardduty_findings_cloudwatch.html) in the *Amazon GuardDuty User Guide*\.

### [AWS Health](https://phd.aws.amazon.com/phd/home#/)<a name="aws-health"></a>

AWS Health provides visibility into the state of your AWS resources, services, and accounts\. It provides information about the performance and availability of resources that affect your applications running on AWS and guidance for remediation\. AWS Health provides this information in a console called the Personal Health Dashboard \(PHD\)\. 

AWS Health directly supports CloudWatch Events notifications\. You configure [CloudWatch Events rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) or [EventBridge rules](https://docs.aws.amazon.com/eventbridge/latest/userguide//create-eventbridge-rule.html) for AWS Health, and specify an Amazon SNS topic mapped in AWS Chatbot\. 

For more information, see [Monitoring AWS Health Events with Amazon CloudWatch Events](https://docs.aws.amazon.com/health/latest/ug//cloudwatch-events-health.html) in the *AWS Health User Guide*\.

### [AWS Security Hub](https://console.aws.amazon.com/securityhub/)<a name="security-hub"></a>

AWS Security Hub provides a comprehensive view of high\-priority security alerts and compliance status across your AWS accounts\. Security Hub aggregates, organizes, and prioritizes security findings from multiple AWS services, including Amazon GuardDuty, Amazon Inspector, and Amazon Macie\. Security Hub reduces the effort of collecting and prioritizing security findings across accounts, from AWS services, and from AWS partner tools\. 

Security Hub supports two types of integration with [CloudWatch Events rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) or [EventBridge rules](https://docs.aws.amazon.com/eventbridge/latest/userguide//create-eventbridge-rule.html), both of which AWS Chatbot supports:
+ **Standard CloudWatch Events**\. [Security Hub automatically sends all findings to CloudWatch Events](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cloudwatch-events.html)\. You can define CloudWatch Events or EventBridge rules that automatically route generated findings to an Amazon S3 bucket, a remediation workflow, or an SNS topic\. Use this method to automatically send all Security Hub findings, or all findings with specific characteristics, to an SNS topic to which AWS Chatbot subscribes\.
+ **Security Hub Custom Actions**\.[ Define custom actions in Security Hub](https://aws.amazon.com/blogs/apn/how-to-enable-custom-actions-in-aws-security-hub/) and configure [CloudWatch Events rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) or [EventBridge rules](https://docs.aws.amazon.com/eventbridge/latest/userguide//create-eventbridge-rule.html) to respond to those actions\. The event rule uses its Amazon SNS topic setting to forward its notifications to the SNS topic to which AWS Chatbot subscribes\. 

### [AWS Systems Manager](https://console.aws.amazon.com/systems-manager/)<a name="system-manager"></a>

AWS Systems Manager lets you view and control your infrastructure on AWS\. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources\. Systems Manager helps you maintain security and compliance by scanning your managed instances, and reporting or taking corrective action on detected policy violations\.

For information about monitoring Systems Manager events with CloudWatch, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-cloudwatch-events.html) in the *AWS Systems Manager User Guide*\. 
