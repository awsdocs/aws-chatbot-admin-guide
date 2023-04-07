# Asking AWS Chatbot questions from chat channels<a name="asking-questions"></a>

You can search and discover information about AWS services and your AWS resources by asking AWS Chatbot natural language questions\. AWS Chatbot answers service related questions directly in your chat channels by providing relevant AWS documentation and support article excerpts\. AWS Chatbot answers resource related questions by using AWS Resource Explorer to search and find your resources\.

**Topics**
+ [AWS service questions](#service-questions)
+ [AWS resource questions](#resource-questions)

## AWS service questions<a name="service-questions"></a>

 You can ask AWS Chatbot questions about AWS services to find relevant product information directly in your chat channels\. For example, you can ask:

`@aws How do I set ec2 autoscaling group's max limit?`

 If AWS Chatbot finds an answer, it presents an excerpt with a direct link to the information source\. 

## AWS resource questions<a name="resource-questions"></a>

 AWS Chatbot uses Resource Explorer to search and discover your resources\. When appropriate, AWS Chatbot shows its search results in a list\. This list shows the top 5 matching resources and includes the ability to filter results further by resource type, AWS Region, and tag\.

### Prerequisities<a name="resource-question-prereq"></a>

 To ask AWS Chatbot resource related questions, you must: 
+ Sign up for Resource Explorer to allow AWS Chatbot to search your resources\. For more information, see [Getting started with Resource Explorer ](https://docs.aws.amazon.com/resource-explorer/latest/userguide/getting-started.html)in the *AWS Resource Explorer User Guide*\. To search your resources across Regions from chat channels, we recommend turning on cross\-Region search by creating an aggregator index\. For more information, see [Turning on cross\-Region search by creating an aggregator index](https://docs.aws.amazon.com/resource-explorer/latest/userguide/manage-aggregator-region.html) in the *AWS Resource Explorer User Guide*\.
**Note**  
You can also create Resource Explorer resources using an AWS CloudFormation template\. For more information, see [Creating Resource Explorer resources with AWS CloudFormation](https://docs.aws.amazon.com/resource-explorer/latest/userguide/cloudformation-support.html) in the *AWS Resource Explorer User Guide*\.
+  Make sure you have active indexes and views with at least one default view in your Region\. Indexes and views allow Resource Explorer to catalog and query your resources\. For more information, see [Terms and concepts for Resource Explorer](https://docs.aws.amazon.com/resource-explorer/latest/userguide/getting-started-terms-and-concepts.html#term-index.html) in the *AWS Resource Explorer User Guide*\. 
+ Add the `AWSResourceExplorerReadOnlyAccess` policy to your channel role or reach appropriate user role, depending on your channel's permissions scheme\.
+ Verify that your channel guardrail policies allow `AWSResourceExplorerReadOnlyAccess` permissions\.

### Commonly asked resource questions<a name="common-resource-questions"></a>

These questions can be asked directly from your chat channels\. Replace words with red text with your own information\.

`@aws What services am I using in Region?`

`@aws What are the resources in my account with tags?`

`@aws What lambda functions do I have?`