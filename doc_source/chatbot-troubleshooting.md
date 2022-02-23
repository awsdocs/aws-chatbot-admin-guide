# Troubleshooting AWS Chatbot<a name="chatbot-troubleshooting"></a>

AWS Chatbot operates with several AWS services, including Amazon CloudWatch, Amazon GuardDuty, and AWS CloudFormation\. If you encounter issues when trying to receive notifications, see the following topic for troubleshooting help\.

## Notifications aren't sent to chat rooms\.<a name="chatbot-subcription-troubleshooting-chat-rooms-not-receiving-notifications"></a>

If you configured your AWS service to send notifications to the Amazon Simple Notification Service \(Amazon SNS\) topics mapped to AWS Chatbot, but the notifications aren't appearing in the chat rooms or channels, try the steps below\.

**Possible causes** 
+ **There is no connectivity\.**

  Test your connectivity and your AWS Chatbot configuration by using the **Send test message button** in the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\. For more information, see [Test notifications from AWS services to Amazon Chime or Slack](getting-started.md#test-notifications)\. 
+ **The bot is not invited to the channel\.**

  Ensure that the AWS Chatbot app \("@aws"\) is added to the Slack channel\. If it hasn't, in Slack, add the AWS Chatbot app by choosing **Add apps** from the channel's **Details** screen\.
+ **The notification's originating service is not supported by AWS Chatbot\.**

  For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.
+ **The SNS topic doesn't have a subscription to AWS Chatbot\.**

  In the Amazon SNS console, go to the **Topics** page, choose the **Subscriptions** tab, and then verify that the topic has a subscription\. If the topic doesn't, open the AWS Chatbot console, open your authorized client, and then look at the **Configured channels** or **Configured webhooks** list\. Add a new channel or webhook configuration, and then add the SNS topic\. Without this configuration, event notifications can't reach the chat rooms\. 
+ **The Amazon SNS topic has server\-side encryption turned on\.**

  If you have server\-side encryption enabled for your Amazon SNS topics, you must include the following section in your AWS KMS key policy\. This gives sending services such as Amazon EventBridge permissions to post events to the encrypted Amazon SNS topics\.

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
+ **Your SNS topic subscription to the AWS Chatbot has the Enable raw message delivery setting enabled\.**

  Don't enable the **Enable raw message delivery** feature for any SNS topic subscriptions to AWS Chatbot\.
+ **The event was throttled\.**

  AWS Chatbot allows for 10 events per second\. If more than 10 events per second are received, any event above 10 is throttled\.
+ **The EventBridge event message content was modified\.**

  AWS Chatbot only delivers notifications with their original EventBridge event message content to chat channels\. If this message content is modified \(such as by using EventBridge [InputTransformers](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-transform-target-input.html)\), AWS Chatbot won't be able to deliver notifications to your chat channels\.

## How can I unsubscribe from AWS Chatbot notifications in a channel or chat room?<a name="unsubscribe-from-chatbot-notifications"></a>

To unsubscribe a channel or chat room from all AWS Chatbot notifications, remove the respective configuration from the AWS Chatbot console\. Otherwise, to identify certain service and notification\-types to unsubscribe from, see [I don't want to receive notifications from certain services anymore\.](#i-dont-want-to-receive-notifications-from-certain-services)

## I don't want to receive notifications from certain services anymore\.<a name="i-dont-want-to-receive-notifications-from-certain-services"></a>

If you want to unsubscribe only some notifications from the channel or chatroom, you can remove specific SNS topics from the AWS Chatbot configuration\. Alternatively, you can remove the specific SNS topics as the event and alarm notification targets from the respective service configurations\. You should also check if you have Amazon EventBridge rules configured for the service event types and remove the specific SNS topics as the rule triggers targets\.

## CloudWatch alarm notifications don't show the graphs from the reporting metrics\.<a name="chatbot-notification-troubleshooting-cloudwatch-alarm-notifications-missing-metrics"></a>

**Possible causes**
+ **The IAM role doesn’t have CloudWatchRead permissions\. **

  In the AWS Chatbot console, create a new role\. This role requires the Notifications permissions policy from the AWS Chatbot console when you configure a new webhook or Slack channel\. You can also edit your IAM role to [add the CloudWatchRead permissions](getting-started.md#editing-iam-roles-for-chatbot) for AWS Chatbot\.
+ **AWS Chatbot doesn't have access to all AWS Regions\.**

  AWS Chatbot may execute API calls from any nearby AWS Region\. If any Region is disabled, you may experience problems with CloudWatch metrics graphs, among other issues\. For more information, see [I get AccessDenied or permissions errors\.](#chatbot-troubleshooting-regions)

## When I set up an SNS topic in the AWS Billing and Cost Management console to forward notifications to the AWS Chatbot, I get a "Please comply with SNS Topic ARN format" error message\.<a name="chatbot-notification-troubleshooting-sns-topic-arn-format-error"></a>

If the AWS Billing and Cost Management console displays an error message for the SNS topic you want to use for notifications, [you can edit the SNS topic's permissions policy so it can forward Budget notifications\.](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-sns-policy.html) 

Do this if you have already configured an SNS topic that has a subscription to AWS Chatbot or you've configured a new SNS topic\. It is not needed if you want to use an Amazon SNS topic that is already configured and working with AWS Billing and Cost Management\. [You can then set up that topic with a subscription to AWS Chatbot](getting-started.md#subscribe-sns-topic)\.

## How do I edit my configuration name?<a name="chatbot-notification-troubleshooting-edit-config-name"></a>

Configuration names can't be edited\. Names must be unique across your account\.

## I get AccessDenied or permissions errors\.<a name="chatbot-troubleshooting-regions"></a>

**Possible causes**
+ **You are missing some IAM permissions or trust relationships\.**

  Make sure you have the correct policies set up by following the instructions found in [Setting up AWS Chatbot](setting-up.md) and [Identity and Access Management for AWS Chatbot](security-iam.md)\.
+ **AWS Chatbot doesn't have access to all AWS Regions\.**

  AWS Chatbot is a global service and may execute API calls from any nearby AWS Region\. If any Region is disabled, you may experience errors\. Make sure the IAM role you set up for AWS Chatbot to assume has access to all Regions\. 

  [Other policy types](security-iam.md#security-iam-other-policies) can limit how IAM roles can be assumed\. If you have set up your AWS Chatbot IAM role to have global access but you're still getting errors, one of these policy types may be the culprit:
  + **AWS Organizations service control policies \(SCPs\)** \- SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit \(OU\) in AWS Organizations\. A service control policy could be overriding the policies you put in place for AWS Chatbot\. See [How SCPs Work](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_about-scps.html) in the *AWS Organizations User Guide*\.
  + **IAM account settings**

    With IAM, you can use AWS Security Token Service \(AWS STS\) to create and provide trusted users with temporary security credentials that can control access to your AWS resources\. When you activate STS endpoints for a Region, AWS STS can issue temporary credentials to users and roles in your account that make an AWS STS request\. Those credentials can then be used in any Region that is enabled by default or is manually enabled\. You must activate the Region in the account where the temporary credentials are generated\. It does not matter whether a user is signed into the same account or a different account when they make the request\. For more information, see [Activating and deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html#sts-regions-activate-deactivate) in the *IAM User Guide*\. 

 If there is a policy in place that prevents access to services in certain Regions, you must change the policy to allow global AWS Chatbot access\.

For example, the policy below allows AWS Chatbot in **us\-east\-2** but denies other services by using a [NotAction](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notaction.html) element\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "NotAction": [
                "chatbot:*"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": [
                        "us-east-2"
                    ]
                }
            }
        }
    ]
}
```

## I get an "Event received is not supported" error\.<a name="chatbot-troubleshooting-unsupported-event-error"></a>

**Possible causes**
+ **The notification's originating service is not supported by AWS Chatbot\.**

  For a list of supported services, see [Using AWS Chatbot with Other AWS Services](related-services.md)\.

## I get an "A valid member name is required" error\.<a name="chatbot-troubleshooting-valid-member-name-required-error"></a>

**Possible causes**
+ **The AWS Chatbot app is not added to the Slack workspace\.**

  Add the AWS Chatbot app to the Slack workspace\. For more information, see the [Getting started guide for AWS Chatbot](getting-started.md)\.

## I get a Slack error saying "You are not authorized to install AWS Chatbot on AWS\."<a name="chatbot-troubleshooting-scope-change"></a>

**Possible causes**
+ **A new scope change requires administrator approval\.**

  There may be a new scope added to the AWS Chatbot Slack application that requires approval by an administrator\. If AWS Chatbot has released a new scope, administrators need to re\-approve the AWS Chatbot Slack application\. Note that the approval is only required for Slack workspaces with an app approval policy\.

  Workspace administrators can check their workspace settings to review and approve new scopes for AWS Chatbot\. For more information about how to approve an app, see [Approve an app for your org](https://slack.com/help/articles/360000281563-Manage-apps-on-Enterprise-Grid#approve-or-restrict-an-app) in the Slack Help Center\.
+ **Installation of the AWS Chatbot Slack app is restricted for your workspace\.**

  This error may appear if the workspace administrator has explicitly restricted the installation of the AWS Chatbot Slack app\.

## Provide feedback<a name="feedback"></a>

You can provide feedback about AWS Chatbot directly from your Amazon Chime chat room, Slack channel, or from the AWS Chatbot console\. To leave feedback from your Amazon Chime chat room or Slack channel, type the following command and replace *comments* with your own information\.

```
@aws feedback comments
```

To leave feedback from the AWS Chatbot console, navigate to the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/) and choose the **Feedback** link at the bottom of the console\. All feedback is sent directly to and reviewed by the AWS Chatbot team\.