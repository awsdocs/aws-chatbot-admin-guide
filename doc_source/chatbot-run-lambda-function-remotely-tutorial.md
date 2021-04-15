# Tutorial: Using AWS Chatbot to run an AWS Lambda function remotely<a name="chatbot-run-lambda-function-remotely-tutorial"></a>

In this tutorial you use AWS Chatbot to run a Lambda function remotely and check the status of the Lambda function using Amazon CloudWatch\. A Lambda function is a self contained block of organized resuable code that you write\. Lambda functions are useful because they are run without provisioning or managing servers\. Additionally, they are only invoked when needed based on your specifications\. There are steps at the end of this tutorial to delete the resources you created\. 





**Topics**
+ [Prerequisites](#prerequisites)
+ [Step 1: Create a Lambda function](#create-lambda-function)
+ [Step 2: Create an SNS topic](#create-sns-topic)
+ [Step 3: Configure a CloudWatch alarm](#configure-cloudwatch-alarm)
+ [Step 4: Configure a Slack client for AWS Chatbot](#create-chatbot-slack-config)
+ [Step 5: Invoke a Lambda function from Slack](#invoke-lambda-function)
+ [Step 6: Test the CloudWatch alarm](#test-cloudwatch-alarm)
+ [Step 7: Clean up resources](#clean-up-resources)

## Prerequisites<a name="prerequisites"></a>

This tutorial assumes that you have some familiarity with the Lambda, AWS Chatbot, and CloudWatch consoles\. 



For more information, see the following topics:
+ [Getting started with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html) in the *AWS Lambda Developer Guide*\.
+ [Setting up AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/setting-up.html) in the *AWS Chatbot Administrator Guide\.*
+ [Getting Set Up with CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingSetup.html) in the *Amazon CloudWatch User Guide\.* 

The AWS Region that you select while setting up these consoles should be the same Region you specify in your Slack channel when your first AWS Command Line Interface \(AWS CLI\) command in [Step 5: Invoke a Lambda function from Slack](#invoke-lambda-function)\. 

## Step 1: Create a Lambda function<a name="create-lambda-function"></a>

In this procedure you create a Lambda function in the console and test it\.

**To create a Lambda function**

1. Sign in to the AWS Management Console and open the Lambda console at [console\.aws\.amazon\.com/lambda](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Choose **Author From Scratch**\.

1. In **Function Name**, enter: **myHelloWorld**

1. Choose **Create Function**\.

1. Copy and paste the following example code into `index.js`\.

   ```
   exports.handler = async (event) => {
         const response = 'Hello World!'
         return response;
      };
   ```

1. Choose **Deploy** and confirm your changes have been deployed by viewing the label next to the **Deploy** button\.

1. Choose **Test**\.

1. In **Event Name**, enter: **myHelloWorld**

1. Choose **Create**\.

1. Choose **Test** and then verify that the **Execution results** tab displays Response: "Hello World\!"

1. Choose **Save**\.

## Step 2: Create an SNS topic<a name="create-sns-topic"></a>

CloudWatch uses Amazon SNS to send notifications\. First, you create an SNS topic and subscribe to it using your email\. Later in the tutorial you use this SNS topic to configure AWS Chatbot\.

**To create an SNS topic**

1. Open the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. In the left navigation pane, choose **Topics**\.

1. Choose **Create Topic**\.

1. Create a topic with the following settings:

   1. **Type** – Standard

   1. **Name** – **myHelloWorldNotifications**

   1. **Display name ** – **myHelloWorld**

1. Choose **Create topic**\.

1. Choose **Create subscription**\.

1. Create a subscription with the following settings:

   1. **Protocol ** – **Email**

   1. **Endpoint ** – Your email address

1. Confirm subscription to the SNS by checking your email and choosing the link\.

## Step 3: Configure a CloudWatch alarm<a name="configure-cloudwatch-alarm"></a>

A CloudWatch alarm monitors your Lambda function and sends a notification if an error occurs\.

**To create a CloudWatch alarm**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Alarms**\.

1. Choose **Create alarm**\.

1. Choose **Select metric**\.

1. Choose **Lambda**\.

1. Choose **By Function Name**\.

1. Choose **myHelloWorld errors**\.

1. Change the following settings:

   1. **Period** – **1 minute**

   1. **Whenever Errors is** **Greater** **than** **0**

   1. **Send notifications to** – **myHelloWorldNotifications**

   1. **Alarm name** – **myHelloWorld\-alarm**

   1. **Alarm description** – **Lambda myHelloWorld alarm**

1. Choose **Create alarm**\.

## Step 4: Configure a Slack client for AWS Chatbot<a name="create-chatbot-slack-config"></a>

You can configure a Slack client using AWS Chatbot to to run different commands in Slack using the AWS CLI\. In this tutorial you use AWS CLI to invoke your Lambda function from Slack\.



**To create a Slack client**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Under **Configure a Chat client** choose **Slack**, and then choose **Configure**\.
**Important**  
When you choose **Configure**, you are momentarily navigated away from the AWS Chatbot console\.

1. In the upper right corner, choose the dropdown list, and then choose the Slack workspace that you want to use with AWS Chatbot\.
**Note**  
There's no limit to the number of workspaces that you can set up for AWS Chatbot, but you must set up each workspace one at a time\.

1. Choose **Allow**\.

1. Choose **Configure new channel**\.

1. Under **Configuration details**, for **Name**, enter **myHelloWorld**\.

1. Under **Channel type**, choose **Private**\.

   1. Navigate to Slack and create a private channel by choosing the **\+** button to the right of **Channels**\.

   1. Choose **Create a channel**\.

   1. Name the channel **myHelloWorld**\.

   1. Choose to make the channel private\.

   1. Choose **Create**\.

   1. When prompted to add people, choose **x**\.

   1. Navigate back to the AWS Chatbot console and enter the private channel ID\.

1. Define the **IAM permissions** that the chatbot uses for messaging your Slack chat room as shown following:

   1. For **Role name**, enter **myHelloWorldRole**\.

   1. For **Policy Templates**, select **Read\-only command permissions** and **Lambda\-invoke command permissions\.**

1. In the SNS topics section, choose the appropriate AWS Region under **Region**\.

1. Under **Topics**, select the **myHelloWorldNotifcations** topic\.

1. Choose **Configure**\.

## Step 5: Invoke a Lambda function from Slack<a name="invoke-lambda-function"></a>

After you configure a chatbot in AWS Chatbot, you can invoke Lambda functions from Slack using AWS CLI syntax\. To interact with AWS Chatbot in Slack, enter **@aws** followed by an AWS CLI command\. For more information, see [Running AWS CLI commands from Slack channels](chatbot-cli-commands.md) in the *AWS Chatbot Administrator Guide\.* 

**To invoke a Lambda function**

1. Invite AWS Chatbot to your channel by doing the following in Slack:

   1. Enter `@AWS`\.

   1. Choose **Invite to Channel\.**
**Tip**  
You only have to invite AWS Chatbot to the channel once\.  
If AWS is not listed as a valid member of the channel, you need to add the AWS Chatbot app to the Slack workspace\. For more information, see the [Getting started guide for AWS Chatbot](getting-started.md)\. 

1. Enter the following command in Slack:

   ```
   @aws lambda invoke --function-name myHelloWorld --region <your region>
   ```
**Important**  
Replace *<your region>* with the same AWS Region you set while using the Lambda, CloudWatch, and AWS Chatbot consoles\. You only need to specify the AWS Region in the channel once when you type your first AWS CLI command in Slack\.
**Tip**  
AWS Chatbot also supports certain simplified AWS CLI syntaxes\. For example, the simplified version of the previous command is shown following:  

   ```
   @aws invoke myHelloWorld --region <your region>
   ```

1. Choose **Yes**\.

1. The following output is shown:

   ```
   ExecutedVersion: $LATEST
   Payload: \"Hello World\"
   StatusCode: 200
   ```

**Troubleshooting**  
If you try to run your Lambda function in Slack and you encounter errors referring to the following permissions, revisit step 8 of the [ Step 4: Configure a Slack client for AWS Chatbot](#create-chatbot-slack-config) procedure and verify that you have the correct permissions assigned to your role:
+ **Lambda\-invoke command permissions**
+ **Read\-only command permissions**

## Step 6: Test the CloudWatch alarm<a name="test-cloudwatch-alarm"></a>

In this step, you update the myHelloWorld function so that it returns an error, which triggers the CloudWatch alarm\. By testing the alarm you can confirm that it's configured correctly and that you can view CloudWatch alarms in Slack \(in addition to logs\)\. 



**To test the CloudWatch alarm**

1. Open the Lambda console [ Functions page](https://console.aws.amazon.com/lambda/)\.

1. Choose **myHelloWorld**\.

1. Copy and paste the following example code into the Lambda function code:

   ```
   exports.handler = async (event) => {
       throw new Error('this is an error');
   };
   ```

1. Choose **Deploy** and confirm your changes have been deployed by viewing the label next to the **Deploy** button\.

1. Return to your Slack channel and then enter the following command: 

   ```
   @aws invoke myHelloWorld
   ```

1. An error appears in your output, and you receive a CloudWatch alarm notification in Slack and an email\. It might take a few minutes for you to receive the notifications\.

1. To view logs, choose **Show logs** or **Show error logs**\.

**Troubleshooting**  
If you don't receive a notification in Slack or an email from CloudWatch, navigate to the CloudWatch console and on the left of the screen\. Under **Alarms**, choose **In alarm** to confirm that your alarm has triggered\. Your alarm name should appear on this page if it has been triggered successfully\.

## Step 7: Clean up resources<a name="clean-up-resources"></a>

You can remove any resources created for this tutorial that you don't want to keep by navigating to the specific service’s console and deleting the resource\.  Removing unwanted or unused resources is beneficial because it lowers overall costs to you\.



**To delete the Lambda function**

1. Open the [Lambda console](https://console.aws.amazon.com/lambda/)\.

1. Choose **myHelloWordFunction**\.

1. Choose **Actions** and then choose **delete**\.

**To delete the CloudWatch alarm**

1. Open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Insufficient**\.

1. Choose myHelloWorld\-alarm by selecting the check box\.

1. Choose **Actions** and then choose **delete**\.

**To delete the AWS Chatbot configuration**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose **Slack\.**

1. Choose the radio button next to the channel you created and then choose **Delete**\.