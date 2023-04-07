# Identity\-based IAM policies for AWS Chatbot<a name="security_iam_service-with-iam-id-based-policies"></a>

A policy is an object in AWS that, when you attach it to an identity, defines their permissions\. When you create a policy to restrict or allow access to a resource, you can use an identity\-based policy\.

You can attach IAM identity\-based policies to IAM entities such as a user in your AWS account, an IAM group, or an IAM role\. You can define allowed or denied actions and resources, and the conditions under which actions are allowed or denied\. AWS Chatbot supports specific actions, resources, and condition keys\.

**Note**  
To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.  
For information about the specific IAM JSON policy elements that AWS Chatbot supports, see [Actions, Resources, and Condition Keys for AWS Chatbot](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awschatbot.html#awschatbot-policy-keys) in the *IAM User Guide*\. 

**Topics**
+ [Identity\-based policies for AWS Chatbot](#security_iam_id-based-policy-examples)
+ [Identity\-based policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Applying AWS Chatbot permissions to an IAM identity](#ChatbotCompleteRoleExample)
+ [Allowing users to view their permissions](#security_iam_id-based-policy-examples-view-own-permissions)

## Identity\-based policies for AWS Chatbot<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users, groups, and roles don't have permission to create or modify AWS Chatbot resources\. They also can't perform tasks using the AWS Management Console or AWS Command Line Interface \(AWS CLI\)\. An IAM administrator can create IAM identity\-based policies that grant entities permission to perform specific console and CLI operations on the resources that they need\. The administrator attaches those policies to the IAM entities that require those permissions\.

**Note**  
In an identity\-based policy, you don't specify the principal who gets the permission \(the `Principal` element\) because the policy gets attached to the entity that needs to use it\.

To learn about all of the elements that you use in a policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\. For information about the specific IAM JSON policy elements that AWS Chatbot supports, see [Actions, Resources, and Condition Keys for AWS Chatbot](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awschatbot.html#awschatbot-policy-keys) in the *IAM User Guide*\. 

### AWS Chatbot actions for identity\-based policies<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Actions in an AWS Chatbot policy use the following prefix before the action\. 

`"Action": [`

` "chatbot:"`

` ]`

For example, to grant a user permission to view the list of all Slack channels using the `DescribeSlackChannels` operation, you include the `chatbot:DescribeSlackChannels` action in the user's policy\. Policy statements must include either an `Action` or `NotAction` element\. AWS Chatbot defines its own set of actions that describe tasks that you can perform with this service\. To see the list of AWS Chatbot actions, see [Actions, Resources, and Condition Keys for AWS Chatbot](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awschatbot.html) in the *IAM User Guide\.*

To specify multiple actions in a single statement, separate them with commas\.

`"Action": [`

` "chatbot:DescribeSlackChannels",`

` "chatbot:DescribeSlackWorkspaces"`

` ]`

**Important**  
Although you can specify multiple actions of like type in a policy using wildcards \(\*\), we strongly discourage doing so\. Follow the practice of granting least privileges and narrowing the permissions necessary for a user to perform their work\.

## Identity\-based policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete AWS Chatbot resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Applying AWS Chatbot permissions to an IAM identity<a name="ChatbotCompleteRoleExample"></a>

The following example of an AWS Chatbot identity\-based policy controls all aspects of Slack chat room configuration\. It grants full read\-only permissions to Amazon CloudWatch and Amazon CloudWatch Logs, and Amazon Simple Notification Service \(Amazon SNS\) topics\. It enables Slack chat room configuration through both the AWS Chatbot console and CLI actions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllChatbotPermissions",
            "Action": [
                "cloudwatch:Describe*",
                "cloudwatch:Get*",
                "cloudwatch:List*",
                "logs:Get*",
                "logs:List*",
                "logs:Describe*",
                "logs:TestMetricFilter",
                "logs:FilterLogEvents",
                "sns:Get*",
                "sns:List*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
            {
            "Sid": "AllSlackPermissions",
            "Effect": "Allow",
            "Action": [
                "chatbot:Describe*",
                "chatbot:UpdateSlackChannelConfiguration",
                "chatbot:CreateSlackChannelConfiguration",
                "chatbot:DeleteSlackChannelConfiguration"
            ],
            "Resource": "*"
        }
    ]
}
```

In this example, `"Resource": "*"` refers to all applicable Slack resources\. You attach the policy to an IAM user, group, or role who needs access to all Slack resources\. 

## Allowing users to view their permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```