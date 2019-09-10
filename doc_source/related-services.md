--------

AWS Chatbot is in beta and is subject to change\.

--------

# Using AWS Chatbot with Other AWS Services<a name="related-services"></a>

AWS Chatbot works with a number of AWS services, including Amazon CloudWatch, AWS Security Hub, and AWS Health\. Services that work with AWS Chatbot use Amazon SNS topics as targets to send event and alarms notifications by email\. You may already have established Amazon SNS topics that send notifications to DevOps and development personnel as e\-mails\. Because AWS Chatbot redirects those Amazon SNS topics' notifications to chat rooms, all that's needed in some cases is to add those Amazon SNS topics to your chat client in the AWS Chatbot console\. Some services require additional configuration\. 

**Topics**
+ [Data Source Services for AWS Chatbot](#AWS::Chatbot::Datasources)

## Data Source Services for AWS Chatbot<a name="AWS::Chatbot::Datasources"></a>

You can set up the following AWS services to target notifications to Amazon Chime or Slack chat rooms\.

### AWS Billing and Cost Management \(for AWS Budget Alerts\)<a name="aws-billing"></a>

AWS Billing and Cost Management helps AWS account holders plan service usage, service costs, and instance reservations\. You do this using several specific types of budgets, which track your unblended costs, subscriptions, refunds, and Reserved Instances\. The service sends its notifications to an Amazon SNS topic\. You then map the Amazon SNS topic in AWS Chatbot to send those notifications to your chat rooms\. For information about setting up Amazon SNS topics for AWS budgets, see [Creating an Amazon SNS Topic for Budget Notifications](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-sns-policy.html) in the *AWS Billing and Cost Management User Guide*\.

### AWS CloudFormation<a name="cloud-formation"></a>

AWS CloudFormation is an infrastructure management service that helps you model and set up Amazon Web Services resources so you can spend less time managing those resources and more time focusing on the applications that you run in AWS\. You create a template that describes all of the AWS resources that you want \(like Amazon EC2 instances or Amazon RDS DB instances\), and AWS CloudFormation takes care of provisioning and configuring those resources for you\. 

AWS Chatbot supports AWS CloudFormation notifications through Amazon SNS topics\. You enable support for AWS Chatbot\-enabled SNS topics by selecting them in each AWS CloudFormation stack configuration\. For more information, see [Setting AWS CloudFormation Stack Options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide//cfn-console-add-tags.html) in the *AWS CloudFormation User Guide*\. 

### Amazon CloudWatch<a name="cloudwatch"></a>

To monitor performance and operating metrics for some AWS services, and send notifications when thresholds are breached, you can create alarms in CloudWatch\. CloudWatch sends an Amazon SNS message or performs an action when the alarm changes state\. The `Alarms` action can send a notification to an Amazon SNS topic that forwards the notification to AWS Chatbot for viewing in chat rooms\. Any metric for any AWS service that can be reported by a CloudWatch alarm can be shared by an Amazon SNS topic through the AWS Chatbot to chat rooms\. This includes alarms for services such as EC2\. For information about setting up Amazon SNS topics to forward CloudWatch alarms, see [Set Up Amazon SNS Notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring//US_SetupSNS.html) in the *Amazon CloudWatch User Guide*\.

Because CloudWatch alarms use Amazon SNS topics to forward alarm notifications, you only need to map the associated Amazon SNS topic to your Slack channel or Amazon Chime webhook configuration in AWS Chatbot\.

[AWS Chatbot also supports CloudWatch *event* notifications from Amazon SNS topics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html)\. AWS Chatbot supports CloudWatch Events only for the supported AWS services that are listed in this topic\.

### AWS Config<a name="aws-config"></a>

AWS Config provides a detailed view of the configuration of AWS resources in your AWS account\. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time\. For AWS Config monitoring, [you configure Amazon CloudWatch Events rules](https://docs.aws.amazon.com/config/latest/developerguide/monitor-config-with-cloudwatchevents.html) to target notifications of specific events from AWS Config to an Amazon SNS topic\. You can then map that topic to AWS Chatbot so you can track the events in chat rooms through those notifications\. AWS Config gives you resource oversight and tracking for auditing and compliance, config change management, troubleshooting, and security analysis\. For more information, see [ Notifications for AWS Config](https://docs.aws.amazon.com/config/latest/developerguide//notifications-for-AWS-Config.html) in the *AWS Config Developer's Guide*\.

### Amazon GuardDuty<a name="aws-guardduty"></a>

Amazon GuardDuty is an AWS security threat monitoring service that detects and reports on potential security threats in your AWS account\. It uses threat intelligence feeds, such as lists of malicious IPs and domains, and machine learning to identify possible unauthorized and malicious activity in your AWS environment\. The reporting mechanism for GuardDuty is called *findings*\. Findings appear in the GuardDuty console and automatically appear as CloudWatch Events\. [You then create Amazon CloudWatch Events rules](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html), so these events appear as notifications to a selected Amazon SNS topic, and then map that SNS topic to AWS Chatbot\. For more information, see [Monitoring Amazon GuardDuty Findings with Amazon CloudWatch Events](https://docs.aws.amazon.com/guardduty/latest/ug//guardduty_findings_cloudwatch.html) in the *Amazon GuardDuty User Guide*\.

### AWS Health<a name="aws-health"></a>

AWS Health provides visibility into the state of your AWS resources, services, and accounts\. It provides information about the performance and availability of resources that affect your applications running on AWS and guidance for remediation\. AWS Health provides this information in a console called the Personal Health Dashboard \(PHD\)\. AWS Health directly supports Amazon CloudWatch Events notifications\. You configure CloudWatch Events rules for AWS Health, and specify an Amazon SNS topic mapped in AWS Chatbot\. For more information, see [Monitoring AWS Health Events with Amazon CloudWatch Events](https://docs.aws.amazon.com/health/latest/ug//cloudwatch-events-health.html) in the *AWS Health User Guide*\.

### AWS Security Hub<a name="security-hub"></a>

AWS Security Hub provides a comprehensive view of high\-priority security alerts and compliance status across your AWS accounts\. Security Hub aggregates, organizes, and prioritizes security findings from multiple AWS services, including Amazon GuardDuty, Amazon Inspector, and Amazon Macie\. Security Hub reduces the effort of collecting and prioritizing security findings across accounts, from AWS services, and from AWS partner tools\. 

Security Hub supports two types of integration with CloudWatch Events, both of which AWS Chatbot supports:
+ **Standard CloudWatch Events**\. [Security Hub automatically sends all findings to CloudWatch Events](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cloudwatch-events.html)\. You can define CloudWatch Events rules that automatically route generated findings to an Amazon S3 bucket, a remediation workflow, or an SNS topic\. Use this method to automatically send all Security Hub findings, or all findings with specific characteristics, to your preferred AWS Chatbot\-enabled SNS topic\.
+ **Security Hub Custom Actions**\.[ Define custom actions in Security Hub](https://aws.amazon.com/blogs/apn/how-to-enable-custom-actions-in-aws-security-hub/) and configure CloudWatch Events rules to respond to those Actions\. The CloudWatch Events rule can use its Amazon SNS topic setting to forward its notifications to an SNS topic with a subscription to AWS Chatbot\. 

### AWS Systems Manager<a name="system-manager"></a>

AWS Systems Manager lets you view and control your infrastructure on AWS\. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources\. Systems Manager helps you maintain security and compliance by scanning your managed instances, and reporting or taking corrective action on detected policy violations\.

AWS Chatbot supports publication of Systems Manager events through CloudWatch events for monitoring and notifications\. You use CloudWatch Events rules with those events, to target SNS topics to send notifications\. You map those SNS topics to chat rooms in the AWS Chatbot console\. For information about monitoring Systems Manager events with CloudWatch, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-cloudwatch-events.html) in the *AWS Systems Manager User Guide*\. 
