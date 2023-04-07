# Getting started with AWS Chatbot<a name="getting-started"></a>

To get started using AWS Chatbot to help manage your AWS infrastructure, follow the steps below to set up AWS Chatbot with chat rooms and Amazon SNS topic subscriptions\.

**Topics**
+ [Setting up AWS Chatbot](#setting-up)
+ [Tutorial: Get started with Slack](slack-setup.md)
+ [Tutorial: Get started with Amazon Chime](chime-setup.md)
+ [Tutorial: Get started with Microsoft Teams](teams-setup.md)
+ [Tutorial: Create or configure an Amazon Simple Notification Service topic as a notification target for Microsoft Teams and AWS CodeStar](teams-codestar.md)
+ [Tutorial: Subscribing an Amazon SNS topic to AWS Chatbot](subscribe-sns-topic.md)
+ [Test notifications from AWS services to chat channels using CloudWatch](test-notifications-cw.md)
+ [Next steps](#next-steps-gettingStarted)

## Setting up AWS Chatbot<a name="setting-up"></a>

To use AWS Chatbot, you authorize a chat client configuration with AWS Chatbot, and optionally configure AWS Chatbot to use an Amazon Simple Notification Service \(Amazon SNS\) topic to deliver notifications to the chat rooms\. Before you can get started, you must complete the following setup tasks\.

### Prerequisites<a name="chatbot-prereqs"></a>

With AWS Chatbot, you can use chat rooms to monitor and respond to events in your AWS Cloud\.

Below are some prerequisites you should have before you begin using AWS Chatbot:
+ You have signed up for AWS and created an AWS Identity and Access Management \(IAM\) administrator user\. If this is your first time using AWS, see [Creating Your First Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
+ You have started using some AWS services\. For more information about AWS services you can use with AWS Chatbot, see [Monitoring AWS services using AWS Chatbot](related-services.md)\.
+ You have administrator privileges with your chosen chat client chat room, tenant, or workspace\.
+ You understand AWS Chatbot's permission schemes\. For more information about AWS Chatbot's permission schemes, see [Understanding permissions](understanding-permissions.md)\.

### Setting up IAM permissions for AWS Chatbot<a name="chatbot-iam"></a>

If you have an existing administrator user, you can access the AWS Chatbot console with no additional permissions\.

If you would like to add AWS Chatbot access to an existing user or group, you can choose from allowed Chatbot actions in IAM\.

If you need to customize an IAM role to work with AWS Chatbot, [you can use the procedure in this topic](understanding-permissions.md#editing-iam-roles-for-chatbot)\.

If you want to allow AWS Chatbot to answer questions about your AWS resources, make sure to add the `AWSResourceExplorerReadOnlyAccess` policy to your IAM role\. You must also make sure that your channel guardrail policies allow `AWSResourceExplorerReadOnlyAccess` permissions\.

**To create a policy to configure AWS Chatbot**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Policies** from the navigation pane\.

1. Choose **Create policy**\.

1. Expand **Service** and find **Chatbot**\.

1. Under **Actions**, expand the **Read** and **Write** sections to see the available actions\.

   **Read** actions include **DescribeChimeWebhookConfigurations**, **DescribeSlackChannelConfigurations**, **DescribeTeamsChannelConfiguration**, and more\.

   **Write** actions include **CreateChimeWebhookConfiguration**, **DeleteChimeWebhookConfiguration**, and more\.

1. After selecting the actions you want to include, choose **Review policy**\.

1. Give your policy a name and description, then choose **Create policy**\. You can now add your new policy to any of your users or groups\.

For more information on updating the permissions of existing users, see [Adding Permissions to a User \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

**Note**  
 AWS Chatbot is a global service that requires access to all AWS Regions\. If there is a policy in place that prevents access to services in certain Regions, you must change the policy to allow global AWS Chatbot access\. For more information about policy types that might limit how IAM roles can be assumed and how to override them, see [Other policy types](security-iam.md#security-iam-other-policies)\. 

### Setting up Amazon SNS topics<a name="chatbot-sns"></a>

To use AWS Chatbot, you must have Amazon SNS topics set up\. If you don't have any Amazon SNS topics yet, follow the steps to get started in [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon Simple Notification Service Developer Guide*\.

If you have server\-side encryption enabled for your Amazon SNS topics, you must give permissions to the sending services in your AWS KMS key policy to post events to the encrypted SNS topics\. The following policy is an example for Amazon EventBridge\.

```
{
  "Sid": "Allow CWE to use the key",
  "Effect": "Allow",
  "Principal": {
    "Service": "events.amazonaws.com"
  },
  "Action": [
    "kms:Decrypt",
    "kms:GenerateDataKey"
  ],
  "Resource": "*"
}
```

In order to successfully test the configuration from the console, your role must also have permission to use the AWS KMS key\.

AWS managed service keys don’t allow you to modify access policies, so you will need AWS KMS/CMK for encrypted SNS topics\. You can then update the access permissions in the AWS KMS key policy to allow the service that sends messages to publish to your encrypted SNS topics \(for example, EventBridge\)\.

## Next steps<a name="next-steps-gettingStarted"></a>

Once you've taken the necessary steps to set up AWS Chatbot, you can get started configuring the chat client of your choice\. For a step\-by\-step guide on how to do this, choose the appropriate tutorial below:
+ [Tutorial: Get started with Slack](slack-setup.md)
+ [Tutorial: Get started with Amazon Chime](chime-setup.md)
+ [Tutorial: Get started with Microsoft Teams](teams-setup.md)