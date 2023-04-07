# Security in AWS Chatbot<a name="security"></a>

 At AWS, cloud security is our highest priority\. As an AWS customer, you benefit from a data center and network architecture that we build to meet the requirements of the most security\-sensitive organizations\.

Security is a shared responsibility between AWS and you\. The [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) describes this as security *of* the cloud and security *in* the cloud:
+ **Security of the cloud** – AWS is responsible for protecting the infrastructure that runs AWS services in the AWS Cloud\. AWS also provides you with services that you can use securely\. Third\-party auditors regularly test and verify our security effectiveness as part of the [AWS compliance programs](http://aws.amazon.com/compliance/programs/)\. To learn about the compliance programs that apply to AWS Chatbot, see [AWS Services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/)\.
+ **Security in the cloud** – Your responsibility is determined by the AWS service that you use\. You are also responsible for other factors including the sensitivity of your data, your company’s requirements, and applicable laws and regulations\. 

This documentation helps you understand how to apply the shared responsibility model when using AWS Chatbot\. The following topics show you how to configure AWS Chatbot to meet your security and compliance objectives\. You also learn how to use other AWS services that help you to monitor and secure your AWS Chatbot resources\. 

**Topics**
+ [Data protection in AWS Chatbot](#per-service-security)
+ [Data privacy in AWS Chatbot](#ml-opt-out)
+ [Identity and Access Management for AWS Chatbot](security-iam.md)
+ [Compliance validation for AWS Chatbot](chatbot-compliance.md)
+ [Resilience in AWS Chatbot](disaster-recovery-resiliency.md)
+ [Infrastructure security in AWS Chatbot](infrastructure-security.md)

## Data protection in AWS Chatbot<a name="per-service-security"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS Chatbot\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual users with AWS IAM Identity Center \(successor to AWS Single Sign\-On\) or AWS Identity and Access Management \(IAM\)\. That way, each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing sensitive data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form text fields such as a **Name** field\. This includes when you work with AWS Chatbot or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form text fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

We also strongly recommend that you don't provide secrets or confidential data to AWS Chatbot\. All commands run using AWS Chatbot come through Slack\. As such, we recommend that you carefully consider what information you send AWS Chatbot through Slack\.

**Note**  
AWS Chatbot doesn't modify any event, alarm, or other reporting data when it forwards Amazon Simple Notification Service \(Amazon SNS\) notifications to chat rooms\. It treats all notifications as read only\. 

## Data privacy in AWS Chatbot<a name="ml-opt-out"></a>

### AWS Chatbot AI powered improvements<a name="ai-improvements"></a>

By default AWS Chatbot stores and processes user data such as AWS Chatbot configurations, notifications, user inputs, AWS Chatbot generated responses, and interaction data\. This data helps AWS Chatbot continuously improve and develop both itself and Artificial Intelligence \(AI\) technologies\. Your data is not shared with any third parties and is protected using sophisticated controls to prevent unauthorized access and misuse\.

If you would like to opt out of sharing your user data, you can toggle **Share user data** from the **Data privacy** page in your **Account settings**\. When you opt out, your data is removed from all AWS Regions\. This includes any previously stored data\.