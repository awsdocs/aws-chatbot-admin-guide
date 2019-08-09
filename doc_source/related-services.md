--------

AWS Chatbot is in beta and is subject to change\.

--------

# Using AWS Chatbot with Other AWS Services<a name="related-services"></a>

AWS Chatbot works with a number of AWS services, including Amazon CloudWatch, AWS Security Hub, and AWS Health\. Services that work with AWS Chatbot use Amazon SNS topics as targets to send event and alarms notifications by email\. You may already have established Amazon SNS topics that send notifications to DevOps and development personnel as e\-mails\. Because AWS Chatbot redirects those Amazon SNS topics' notifications to chat rooms, in most cases, all you have to do is add those Amazon SNS topics to your chat clients in the AWS Chatbot console\. Some services may require additional configuration\. 

## Data Source Services for AWS Chatbot<a name="data-source-services"></a>

You can set up the following AWS services to target notifications to Amazon Chime or Slack chat rooms\.

** AWS Billing and Cost Management \(for AWS Budget Alerts\)**

AWS Billing and Cost Management helps AWS account holders plan service usage, service costs, and instance reservations\. You do this using several specific types of budgets, which track your unblended costs, subscriptions, refunds, and Reserved Instances\. The service sends its notifications to an Amazon SNS topic\. You then set the Amazon SNS topic to send those notifications through AWS Chatbot to your chat rooms\. For information about setting up Amazon SNS topics for AWS budgets, see [Creating an Amazon SNS Topic for Budget Notifications](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-sns-policy.html) in the* AWS Billing and Cost Management User Guide*\.

Because Billing and Cost Management only uses Amazon SNS topics to forward notifications, the only step you need to take to send Billing and Cost Management notifications to your chat rooms is to add the associated Amazon SNS topic to your Slack channel or Amazon Chime webhook configuration in AWS Chatbot\.

**AWS CloudFormation**

AWS CloudFormation is an infrastructure management service that helps you model and set up Amazon Web Services resources so you can spend less time managing those resources and more time focusing on the applications that you run in AWS\. You create a template that describes all of the AWS resources that you want \(like Amazon EC2 instances or Amazon RDS DB instances\), and AWS CloudFormation takes care of provisioning and configuring those resources for you\. 

AWS Chatbot supports AWS CloudFormation notifications through Amazon SNS topics\. AWS CloudFormation can act as a data source of events to your chat rooms for teams managing or developing AWS CloudFormation infrastructure\.

 **Amazon CloudWatch** 

To monitor performance and operating metrics for AWS services and send notifications when thresholds are breached, you can create alarms in CloudWatch\. CloudWatch sends an Amazon SNS message or performs an action when the alarm changes state\. The `Alarms` action can send a notification to an Amazon SNS topic that forwards the notification to AWS Chatbot for viewing in chat rooms\. For information about setting up Amazon SNS topics for AWS services, see [Set Up Amazon SNS Notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring//US_SetupSNS.html) in the *Amazon CloudWatch User Guide*\.

Because CloudWatch alarms and events use Amazon SNS topics to forward notifications, the only step needed to send CloudWatch notifications to your chat rooms is to map the associated Amazon SNS topic to your Slack channel or Amazon Chime webhook configuration in AWS Chatbot\. AWS Chatbot also supports CloudWatch *event* notifications from Amazon SNS topics for the supported AWS services listed in this topic\.

 **AWS Config** 

AWS Config provides a detailed view of the configuration of AWS resources in your AWS account\. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time\. For monitoring, you configure AWS Config to target notifications to an Amazon SNS topic, so that when users update resources, you can track the changes through those notifications\. AWS Config gives you resource oversight and tracking for auditing and compliance, config change management, troubleshooting, and security analysis\. For more information, see [ Notifications for AWS Config](https://docs.aws.amazon.com/config/latest/developerguide//notifications-for-AWS-Config.html) in the *AWS Config Developer's Guide*\.

**Note**  
Because AWS Config uses Amazon SNS topics to forward notifications, the only step needed to send AWS Config notifications to your chat rooms is to map the associated Amazon SNS topic to your Amazon Chime webhook or Slack channel configuration in the AWS Chatbot service\.

 **Amazon GuardDuty**

Amazon GuardDuty is an AWS security threat monitoring service that detects and reports on potential security threats in your AWS account\. It uses threat intelligence feeds, such as lists of malicious IPs and domains, and machine learning to identify possible unauthorized and malicious activity in your AWS environment\. The reporting mechanism for GuardDuty is called *findings*\. Findings appear in the GuardDuty console and automatically appear as CloudWatch Events\. These events can appear as notifications through Amazon SNS topics\. To include findings in your Amazon Chime or Slack chat rooms, configure your GuardDuty findings with the Amazon SNS topic configured for AWS Chatbot as their target\. For more information, see [Monitoring Amazon GuardDuty Findings with Amazon CloudWatch Events](https://docs.aws.amazon.com/guardduty/latest/ug//guardduty_findings_cloudwatch.html) in the *Amazon GuardDuty User Guide*\.

 **AWS Health** 

AWS Health provides visibility into the state of your AWS resources, services, and accounts\. It provides information about the performance and availability of resources that affect your applications running on AWS and guidance for remediation\. AWS Health provides this information in a console called the Personal Health Dashboard \(PHD\)\. AWS Health directly supports Amazon CloudWatch Events notifications\. You configure CloudWatch Events rules for AWS Health, and specify an Amazon SNS topic mapped in AWS Chatbot as its target\. For more information, see [Monitoring AWS Health Events with Amazon CloudWatch Events](https://docs.aws.amazon.com/health/latest/ug//cloudwatch-events-health.html) in the *AWS Health User Guide*\.

**AWS Security Hub**

AWS Security Hub provides a comprehensive view of high\-priority security alerts and compliance status across your AWS accounts\. Security Hub aggregates, organizes, and prioritizes security findings from multiple AWS services, including Amazon GuardDuty, Amazon Inspector, and Amazon Macie\. It reduces the effort of collecting and prioritizing security findings across accounts, from AWS services, and from AWS partner tools\. You enable notifications from Security Hub by creating Security Hub Custom Actions, and configuring CloudWatch alarms to respond to those Custom Actions from Security Hub\. CloudWatch uses the Amazon SNS topic configured in the alarm's action settings to forward those notifications to chat rooms through AWS Chatbot\. The Amazon SNS topic should be configured in AWS Chatbot for your Slack or Amazon Chime chat rooms\.

**AWS Systems Manager **

AWS Systems Manager lets you view and control your infrastructure on AWS\. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources\. Systems Manager helps you maintain security and compliance by scanning your managed instances, and reporting or taking corrective action on detected policy violations\.

AWS Chatbot supports Systems Manager events that are published through CloudWatch events for monitoring and notifications\. You use CloudWatch events *rules* with those events, to target SNS topics to send notifications\. You map those SNS topics to chat rooms in the AWS Chatbot console\. For information about monitoring Systems Manager events with CloudWatch, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-cloudwatch-events.html) in the *AWS Systems Manager User Guide*\. 