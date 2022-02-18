# Tutorial: Creating an Amazon EventBridge rule that sends notifications to AWS Chatbot<a name="create-eventbridge-rule"></a>

AWS Chatbot currently supports notifications for all service events that are handled by Amazon EventBridge\. When you create a rule for events, you tell Amazon EventBridge what action to take for events that match the rule\. In this tutorial, you create a rule to use AWS Chatbot to generate an Amazon SNS topic notification that will appear in your Slack channel or Amazon Chime chatroom\.

**Tip**  
You might create an Amazon EventBridge rule that unintentionally sends too many notifications to your chat channels\. This typically happens with EventBridge rules that trigger on API calls via AWS CloudTrail\. To better control the number of notifications you generate, write your EventBridge rules with event pattern based filtering\. For more information about EventBridge event patterns, see [Content\-based filtering in Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns-content-based-filtering.html) in the *Amazon EventBridge User Guide*\.

## Prerequisites<a name="prerequisites"></a>

For this tutorial, you need a Slack or Amazon Chime client for AWS Chatbot\. For more information, see [Getting started with AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/getting-started.html) in the *AWS Chatbot Administrator Guide*\.

You also need to set up Amazon Simple Notification Service\. If you don't have any Amazon SNS topics yet, follow the steps in [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon Simple Notification Service Developer Guide*\.

 For more information about Amazon EventBridge, see [What Is Amazon Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html) in the *Amazon Amazon EventBridge User Guide*\. 

## Create an Amazon EventBridge Rule<a name="creating-ebrule"></a>

Receiving notifications about events of interest in your Slack channel or Amazon Chime chat room is a convenient way to monitor service processes\. In this tutorial, you create an Amazon EventBridge rule to use AWS Chatbot to generate an Amazon SNS topic notification that will appear in your Slack channel or chat room\. It is important to note that you only receive notifications for Amazon SNS topics that are defined in your AWS Chatbot client\.

**To create an Amazon EventBridge rule**

1. Open the Amazon Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/\.]()

1. In the navigation pane, choose **Rules**\.

1. Choose ** Create rule**\.

1. Enter a name and description for the rule\. A rule must have a unique name from other rules in the same Region\.

1. For **Define pattern**, choose **Event Pattern**\.

1. For **Event matching pattern**, choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **AWS**\.
**Important**  
Choosing **All Events** as your service provider can result in increased costs and notifications\. 
**Note**  
Currently only AWS Services are supported\.

1. For **Service name**, choose the name of the service that emits the event\.

1. For **Event type**, choose the events you are interested in\.
**Tip**  
You can edit event patterns by choosing **Edit** and then choosing **Save**\.

1. In **Select targets**, set your **Target** as an **SNS Topic**

1. For **Topic**, choose the appropriate topic\.

1. \(Optional\) To add additional targets, choose **Add target**\.

1. Choose **Create**\.

After the rule is created, you can view, edit, or delete it in the console under **Rules**\. When an event occurs that matches the rule, you receive a notification in your Slack channel or Amazon Chime chat room from AWS Chatbot\.

For information about testing your rule, see [Test notifications from AWS services to Amazon Chime or Slack](getting-started.md#test-notifications)\.

## Delete an Amazon EventBridge rule<a name="deleting-ebrule"></a>

You can remove any resources created for this tutorial by navigating to the Amazon EventBridge console and deleting the resource\.

**To delete or disable an Amazon EventBridge rule**
+ To delete or disable an Amazon EventBridge rule, see [Deleting or Disabling a Rule](https://docs.aws.amazon.com/eventbridge/latest/userguide/delete-or-disable-rule.html) in the *Amazon Amazon EventBridge User Guide*\.
