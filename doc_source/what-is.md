--------

AWS Chatbot is in beta and is subject to change\.

--------

# What Is AWS Chatbot?<a name="what-is"></a>

AWS Chatbot is an AWS service that enables DevOps and software development teams to use Amazon Chime and Slack chat rooms to monitor and respond to operational events in their AWS Cloud\. AWS Chatbot processes AWS service notifications from Amazon Simple Notification Service \(Amazon SNS\), and forwards them to Amazon Chime and Slack chat rooms so teams can analyze and act on them immediately, regardless of location\.

You can also run AWS CLI commands in Slack channels, and file AWS Support cases from the Slack screen\.

**Topics**
+ [Features of AWS Chatbot](#chatbot-benefits)
+ [How AWS Chatbot Works](#chatbot-works)
+ [Regions and Endpoints for AWS Chatbot](#chatbot-regions)
+ [AWS Chatbot Requirements](#chatbot-requirements)
+ [Accessing AWS Chatbot](#chatbot-access)
+ [AWS Chatbot Preview Limitations](#chatbot-limitations)

## Features of AWS Chatbot<a name="chatbot-benefits"></a>

AWS Chatbot enables ChatOps for AWS\. *ChatOps* speeds software development and operations by enabling DevOps teams to use chat clients and chatbots to communicate and execute tasks\. AWS Chatbot notifies chat users about events in their AWS services, so teams can collaboratively monitor and resolve issues in real time, instead of addressing emails from their SNS topics\. AWS Chatbot also allows you to format incident metrics from Amazon CloudWatch as charts for viewing in chat notifications\.

## How AWS Chatbot Works<a name="chatbot-works"></a>

AWS Chatbot uses Amazon Simple Notification Service \(Amazon SNS\) topics to send event and alarm notifications from AWS services to Slack and Amazon Chime chat rooms\. Slack and Amazon Chime users map the SNS topics to their Slack channels or Amazon Chime webhooks\. For Slack, after the Slack administrator approves AWS Chatbot support for the Slack workspace, anyone in the workspace can add AWS Chatbot to their Slack channels\. For Amazon Chime, users with AWS Identity and Access Management \(IAM\) permissions to use Amazon Chime can add AWS Chatbot to their webhooks\.

You use the AWS Chatbot console to configure Amazon Chime and Slack clients to receive notifications from SNS topics\. 

AWS Chatbot supports a number of AWS services, including Amazon CloudWatch, AWS Billing and Cost Management, and AWS Security Hub\. For a complete list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

## Regions and Endpoints for AWS Chatbot<a name="chatbot-regions"></a>

AWS Chatbot is available in all commercial AWS Regions\. It supports using all supported AWS services in the Regions where they are available\.

## AWS Chatbot Requirements<a name="chatbot-requirements"></a>

To use AWS Chatbot, you need the following:
+ An AWS account to associate with Amazon Chime or Slack chat clients during AWS Chatbot setup\. 
+ Administrative privileges for your Slack workspace or Amazon Chime chat room\. You can be the Slack workspace owner or have the ability to work with workspace owners to get approval for installing AWS Chatbot\.
+ Familiarity with AWS Identity and Access Management \(IAM\) and IAM roles and policies\. For more information about IAM, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/)
+ Experience with the AWS services supported by AWS Chatbot, including experience configuring those services to subscribe to Amazon Simple Notification Service \(Amazon SNS\) topics to send notifications\. For information about supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

To access Amazon CloudWatch metrics, AWS Chatbot requires an AWS Identity and Access Management \(IAM\) role with a permissions policy and a trust policy\. You create this IAM role, with the required policies, [using the AWS Chatbot console](https://us-east-2.console.aws.amazon.com/chatbot/home?region=us-east-2#/chat-clients)\. You can use an existing IAM role, but it must have the required policies\.

For testing, we recommend using the role that you create with the AWS Chatbot console\. To use an existing IAM role, see [Configuring an IAM Role for AWS Chatbot](getting-started.md#AWS::Chatbot::Role)\.

## Accessing AWS Chatbot<a name="chatbot-access"></a>

You access and configure AWS Chatbot with its console\.

## AWS Chatbot Preview Limitations<a name="chatbot-limitations"></a>

The AWS Chatbot Preview supports the AWS services described in [Using AWS Chatbot with Other AWS Services](related-services.md)\. It doesn't support the following:
+ Sending SNS notifications from unsupported AWS services
+ Passing through arbitrary text using SNS topics