# What is AWS Chatbot?<a name="what-is"></a>

AWS Chatbot is an AWS service that enables DevOps and software development teams to use messaging program chat rooms to monitor and respond to operational events in their AWS Cloud\. AWS Chatbot processes AWS service notifications from Amazon Simple Notification Service \(Amazon SNS\), and forwards them to chat rooms so teams can analyze and act on them immediately, regardless of location\.

You can also run AWS CLI commands in chat channels using AWS Chatbot\.

**Topics**
+ [Features of AWS Chatbot](#chatbot-benefits)
+ [How AWS Chatbot works](#chatbot-works)
+ [Regions and quotas for AWS Chatbot](#chatbot-regions)
+ [AWS Chatbot requirements](#chatbot-requirements)
+ [Accessing AWS Chatbot](#chatbot-access)

## Features of AWS Chatbot<a name="chatbot-benefits"></a>

AWS Chatbot enables ChatOps for AWS\. *ChatOps* speeds software development and operations by enabling DevOps teams to use chat clients and chatbots to communicate and execute tasks\. AWS Chatbot notifies chat users about events in their AWS services, so teams can collaboratively monitor and resolve issues in real time, instead of addressing emails from their SNS topics\. AWS Chatbot also allows you to format incident metrics from Amazon CloudWatch as charts for viewing in chat notifications\.

Important features of the AWS Chatbot service include the following:
+ **Supports Amazon Chime, Microsoft Teams, and Slack** – You can add AWS Chatbot to your Amazon Chime chat rooms, Microsoft Teams channel, or Slack channel in just a few clicks\.
+ **Predefined AWS Identity and Access Management \(IAM\) policy templates** – AWS Chatbot provides chat room\-specific permission controls through AWS Identity and Access Management \(IAM\)\. AWS Chatbot’s predefined templates make it easy to select and set up the permissions you want associated with a given channel or chat room\.
+ **Receive notifications** – Use AWS Chatbot to receive notifications about operational incidents and other events from supported sources, such as operational alarms, security alerts, or budget deviations\. To set up notifications in the AWS Chatbot console, you simply choose the channels or chat rooms you want to receive notifications and then choose which Amazon Simple Notification Service \(Amazon SNS\) topics should trigger notifications\.
+ **Monitor and manage AWS resources through the AWS CLI with Microsoft Teams and Slack** – AWS Chatbot supports CLI commands for most AWS services, making it easy to monitor and manage your AWS resources from your chat clients on desktop and mobile devices\. Your teams can retrieve diagnostic information in real\-time, change your AWS resources, run AWS SM runbooks, and start long running jobs from a centralized location\. AWS Chatbot commands use the standard AWS Command Line Interface syntax\.
+ **Search and discover AWS information** – You can search and discover information about AWS services and your AWS resources by asking AWS Chatbot natural language questions\. The answers provided in your chat channels are pulled directly from your AWS enviornments, AWS product documentation, and support articles\. This makes it easier to locate your resources, find product information, and troubleshoot issues\.

## How AWS Chatbot works<a name="chatbot-works"></a>

AWS Chatbot uses Amazon Simple Notification Service \(Amazon SNS\) topics to send event and alarm notifications from AWS services to your chat channels\. Once an SNS topic is associated with a configured chat client, events and alarms from various services are processed and notifications are delivered to the specified chat channels and webhooks\. For Microsoft Teams and Slack, after an administrator approves AWS Chatbot support for the workspace or tenant, anyone in the workspace or team can add AWS Chatbot to their chat channels\. For Amazon Chime, users with AWS Identity and Access Management \(IAM\) permissions to use Amazon Chime can add AWS Chatbot to their webhooks\. You use the AWS Chatbot console to configure chat clients to receive notifications from SNS topics\. 

AWS Chatbot supports a number of AWS services, including Amazon CloudWatch, AWS Billing and Cost Management, and AWS Security Hub\. For a complete list of supported services, see [Monitoring AWS services](related-services.md)\.

You can also run AWS CLI commands directly in chat channels using AWS Chatbot\. You can retrieve diagnostic information, configure AWS resources, and run workflows\. To run a command, AWS Chatbot checks that all required parameters are entered\. If any are missing, AWS Chatbot prompts you for the required information\. AWS Chatbot then confirms if the command is permissible by checking the command against what is allowed by the configured IAM roles and the channel guardrail policies\. For more information, see [Running AWS CLI commands from chat channels](chatbot-cli-commands.md) and [Understanding permissions](understanding-permissions.md)\.

## Regions and quotas for AWS Chatbot<a name="chatbot-regions"></a>

AWS Chatbot is a global service and can be used in all commercial AWS Regions\. You can combine Amazon SNS topics from multiple Regions in a single AWS Chatbot configuration\.

AWS Chatbot doesn't currently support service endpoints and there are no adjustable quotas\. For more information about AWS Chatbot AWS Region availability and quotas, see [AWS Chatbot endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/chatbot.html)\. AWS Chatbot supports using all supported AWS services in the Regions where they are available\.

## AWS Chatbot requirements<a name="chatbot-requirements"></a>

To use AWS Chatbot, you need the following:
+ An AWS account to associate with Amazon Chime, Microsoft Teams, or Slack chat clients during AWS Chatbot setup\. 
+ Administrative privileges for your Amazon Chime chat room, Microsoft Teams tenant, or Slack workspace\. You can be the Slack workspace owner or have the ability to work with workspace owners to get approval for installing AWS Chatbot\.
+ Familiarity with AWS Identity and Access Management \(IAM\) and IAM roles and policies\. For more information about IAM, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/) in the *IAM User Guide*\.
+ Experience with the AWS services supported by AWS Chatbot, including experience configuring those services to subscribe to Amazon Simple Notification Service \(Amazon SNS\) topics to send notifications\. For information about supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

To access Amazon CloudWatch metrics, AWS Chatbot requires an AWS Identity and Access Management \(IAM\) role with a permissions policy and a trust policy\. You create this IAM role, with the required policies, [using the AWS Chatbot console](https://us-east-2.console.aws.amazon.com/chatbot/home?region=us-east-2#/chat-clients)\. You can use an existing IAM role, but it must have the required policies\.

## Accessing AWS Chatbot<a name="chatbot-access"></a>

You access and configure AWS Chatbot through the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

You can also access the AWS Chatbot app from the [Slack app directory](https://amzn-aws.slack.com/apps/A6L22LZNH-aws-chatbot?settings=1)\.