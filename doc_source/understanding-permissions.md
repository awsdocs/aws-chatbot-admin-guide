# Understanding AWS Chatbot permissions<a name="understanding-permissions"></a>

AWS Chatbot requires an AWS Identity and Access Management \(IAM\) role to perform actions\. Actions you can perform in your chat channels include running commands and responding to interactive messages\. AWS Chatbot uses channel roles, user roles, and channel guardrail policies to control the actions channel members can take\. What your users can do is the intersection of your guardrail policies and what is allowed by their roles\.

**Topics**
+ [Role setting](#role-settings)
+ [Channel guardrail policies](#channel-guardrails)
+ [Non\-supported operations](#forbidden-permissions)
+ [Managing user roles](manage-user-roles.md)
+ [Editing an IAM role for AWS Chatbot](#editing-iam-roles-for-chatbot)
+ [Managing IAM role permissions for running commands](#iam-policies-for-slack-channels-cli-support)
+ [Protection policy](#cbt-protection-policy)

## Role setting<a name="role-settings"></a>

### Channel role<a name="channel-role"></a>

 A channel role gives all channel members the same permissions\. This is useful if your channel members are similar users or they typically perform the same actions\. You can use an existing role as your channel role or you can create a new role using templates\. If you use a channel role, your channel members can still choose their own user roles\. Your channel role is restricted by your guardrail policies\. You can set your channel role in channel configurations from the AWS Chatbot console\. 

#### Channel role templates<a name="channel-template"></a>

 There are five templates that can be used to create a channel role: 
+ Notification permissions
+ Read\-only command permissions
+ Lambda\-invoke command permissions
+ AWS Support command permissions
+ Incident Manager permissions

 You can use any and all combinations of these templates to suit your needs\. For example, if you want to create a configuration that only delivers notifications, choose **Notification permissions** as your policy template\. If you want your channel members to run read\-only commands exclusively and you want notifications to be delivered, choose **Read\-only command permissions** and **Notification permissions** as your policy templates\. 

### User roles<a name="user-roles"></a>

 User roles require channel members to choose their own roles\. As a result, different users in your channel can have different permissions\. If you have a diverse set of channel members or you don't want new channel members to perform actions as soon as they join your channel, user roles are appropriate\. Under this schema, your channel members must have applied a user role to perform actions\. When channel members apply a user role, it is mapped to their chat client ID\. Administrators can unmap user roles from chat client IDs in the AWS Chatbot console\. Your channel member's actions are limited by your guardrail policies, despite what user roles they may have applied\. For more information on managing user roles, see [Managing user roles](manage-user-roles.md)\. 

#### User role requirement<a name="role-reqs"></a>

 Administrators can require user roles for all current channel members and channels and all channels created in the future by enabling a user role requirement in the AWS Chatbot console\. Individual channels can't override this requirement\. This can be done at the account level in **User permissions**, if you want to require every workspace and channel to use user roles\. It can also be done at the channel configuration level wherein a channel level administrator can enable the user role requirement\. 

**Note**  
This feature is enforced at the account level\.

## Channel guardrail policies<a name="channel-guardrails"></a>

Guardrail policies provide detailed control over what actions are available to your channel members and what actions AWS Chatbot can perform on your behalf\. They constrain and take precedence over both user roles and channel roles\. For example, if a user has a user role that allows administrator access, and they belong to a channel where the channel role or the guardrail policies limit permissions on one or more services, the user will have less than administrator\-level access\. You can set, view, and edit your guardrail policies in the AWS Chatbot console\. If you had an AWS Chatbot configuration before the expansion of available commands on 11/28/2021, you may have a protection policy applied as one of your guardrail policies\. 

**Note**  
AWS Service Roles IAM policies can't be used as guardrail policies\.

## Non\-supported operations<a name="forbidden-permissions"></a>

AWS Chatbot does not support running commands for the following operations:
+  Chatbot 
  + All Operations
+  Amazon Cognito user pools 
  + All Operations
+  IAM 
  + All Operations
+  AWS Key Management Service 
  + All Operations
+  Amazon SimpleDB 
  + All Operations
+  Secrets Manager 
  + All Operations
+  AWS IAM Identity Center \(successor to AWS Single Sign\-On\) 
  + All Operations
+ AWS Security Token Service
  +  All Operations 
+ AWS AppSync
  +  ListApiKeys 
+  AWS CodeCommit 
  + GetFile
  + GetCommit
  + GetDifferences
+ Amazon Connect
  + GetFederationToken
+ Amazon DynamoDB
  + BatchGetItem
  + GetItem
+ Amazon EC2
  + GetPasswordData
+ Amazon ECR
  + GetAuthorizationToken
  + GetLogin
+ Amazon GameLift
  + RequestUploadCredentials
  + GetInstanceAccess
+ Lightsail
  + DownloadDefaultKeyPair
  + GetInstanceAccessDetail
  + GetKeyPair
  + GetKeyPairs
  + UpdateRelationalDatabase
+ Amazon Redshift
  + GetClusterCredentials
+ StorageGateway
  + DescribeChapCredentials
+ Amazon S3
  + GetBucketPolicy
  + GetObject
  + HeadObject
+ Snowball
  + GetJobUnlockCode

## Editing an IAM role for AWS Chatbot<a name="editing-iam-roles-for-chatbot"></a>

You can create new IAM roles in the AWS Chatbot console, which provides a convenient way to deploy the AWS Chatbot service\. You associate these roles with your chat channels or Amazon Chime webhooks\. The AWS Chatbot console does not allow editing of IAM roles, including any roles that you've already created in the AWS Chatbot console\. 

**Note**  
AWS requires that you use the IAM console to edit IAM roles\. If you create roles in the AWS Chatbot console, you must use the IAM console to edit them\. This might happen, for example, when you are using the AWS Chatbot service and a new release comes out that supports new features\.

Use the IAM console to edit AWS Chatbot roles\. You can use the entire set of IAM console features to specify permissions for your AWS Chatbot users\.

**To edit roles**

1. Open the AWS Chatbot console at [https://console.aws.amazon.com/chatbot/](https://console.aws.amazon.com/chatbot/)\.

1. Choose the configured client, and choose the name of the configured channel or webhook\. 

1. Choose the role you want to edit\. When you choose a role, the IAM console opens, automatically showing role configuration page, with the Permissions tab displaying the selected role\. 
**Note**  
You can attach AWS managed policies and customer managed policies\. AWS Chatbot roles support both types of IAM policies\.

------
#### [ Channel role/IAM role ]

   1. On this page, choose the Channel role or IAM role and The IAM console opens, automatically showing role configuration page, with the **Permissions** tab displaying the selected role\. 

   1. Choose **Attach Policies**\.

------
#### [ User roles ]

   1. Choose the **User role** tab\.

   1. Choose **Edit**\.

   1. Choose **Selected role information**\.

------

1. Choose the name of the policy that you want\. You can use the **Search** box to search for the policy by name or by a partial string of characters\. For example, all IAM policies associated with AWS Chatbot include the character string **Chatbot** as part of the policy name\. Following are the preconfigured customer managed policies available for AWS Chatbot:
   + **AWS\-Chatbot\-NotificationsOnly\-Policy**
   + **AWS\-Chatbot\-ReadOnly\-Commands\-Policy**

   You can use these policies to create your own policies that are less permissive and specify the resources their users can access in their roles\. You can also substitute these custom policies for the ones listed here\.

1. You can attach any of three AWS managed policies to any role\. You can use these policies as templates to create your own policies\.
   + **ReadOnlyAccess**
   + **CloudWatchReadOnlyAccess**
   + **AWSSupportAccess**

   The **ReadOnlyAccess** policy is automatically attached to any role that you create in the AWS Chatbot console\. 

   The **AWSSupportAccess** policy is the only AWS managed policy that appears in the AWS Chatbot console when you configure new roles there\.

   You can use these policies to create your own policies that are less permissive and specify the resources their users can access\. You can substitute these custom policies for the ones listed here\.

1. Choose each of the policies that you want to attach to the role and choose **Attach policy**\. If needed, use the Search box to locate the policies you're looking for\.

   After you click **Attach policy**, the role's **Permissions** page opens and shows the change in the **Permissions** list\.

**Note**  
For more information about the customer managed policies and AWS managed policies described in this section, see [IAM Policies for AWS Chatbot](chatbot-iam-policies.md)\.  
For more information about editing IAM policies, see [Editing IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-edit.html)\. Exercise caution at all times when editing policies, and avoid overwriting existing customer managed policies\.

## Managing IAM role permissions for running commands<a name="iam-policies-for-slack-channels-cli-support"></a>

With AWS Identity and Access Management \(IAM\), you can use *identity\-based policies*, which are JSON permissions policy documents, and attach them to an *identity*, such as a user, role, or group\. These policies work with your guardrail policies to control what actions a user can perform\. AWS Chatbot provides three IAM policies in the AWS Chatbot console that you can use to set up AWS CLI commands support for chat channels\. Those policies include:
+ **ReadOnly Command Permissions**
+ **Lambda\-Invoke Command Permissions**
+ **AWS Support Command Permissions**

You can use any or all of these policies, based on your organization's requirements\. To use them, create a new channel IAM role in your channel configuration using the AWS Chatbot console, and attach the policies there\. You can also attach the policies to the AWS Chatbot IAM roles using the IAM console\. The policies simplify AWS Chatbot role configuration and enable you to set up quickly\. 

You can use these IAM policies as templates to define your own policies\. For example, all policies described here use a wildcard \("\*"\) to apply the policy's permissions to all resources:

```
               "Resource": [
                "*"
            ]
```

You can define custom permissions in a policy to limit actions to specific resources in your AWS account\. These are called *resource\-based permissions*\. For more information on defining resources in a policy, see the section [IAM JSON Policy Elements: Resource](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_resource.html) in the *IAM User Guide*\.

For more information on these policies, see [Configuring an IAM Role for AWS Chatbot](#editing-iam-roles-for-chatbot)\.

### Using the AWS Chatbot read\-only command permissions policy<a name="about-readonlycommand-chatbot-policy"></a>

The AWS Chatbot **ReadOnly Command Permissions** policy controls access to several important AWS services, including IAM, AWS Security Token Service \(AWS STS\), AWS Key Management Service \(AWS KMS\), and Amazon S3\. It disallows all IAM operations when using AWS commands in Microsoft Teams and Slack\. When you use the **ReadOnly Command Permissions** policy, you allow or deny the following permissions to users who run commands in chat channels: 
+ IAM \(Deny All\)
+ AWS KMS \(Deny All\)
+ AWS STS \(Deny All\)
+ Amazon Cognito \(allows Read\-Only, denies `GetSigningCertificate` commands\)
+ Amazon EC2 \(allows Read\-Only, denies `GetPasswordData` commands\)
+ Amazon Elastic Container Registry \(Amazon ECR\) \(allows Read\-Only, denies `GetAuthorizationToken` commands\)
+ Amazon GameLift \(allows Read\-Only, denies requests for credentials and `GetInstanceAccess` commands\)
+ Amazon Lightsail \(allows List, Read, denies several key pair operations and `GetInstanceAccess`\)
+ Amazon Redshift \(denies `GetClusterCredentials` commands\)
+ Amazon S3 \(allows Read\-Only commands, denies `GetBucketPolicy` commands\)
+ AWS Storage Gateway \(allows Read\-Only, denies `DescribeChapCredentials` commands\)

The **ReadOnly Command Permissions** policy JSON code is shown following:

```
{
   "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "iam:*",
                "kms:*",
                "sts:*",
                "cognito-idp:GetSigningCertificate",
                "ec2:GetPasswordData",
                "ecr:GetAuthorizationToken",
                "gamelift:RequestUploadCredentials",
                "gamelift:GetInstanceAccess",
                "lightsail:DownloadDefaultKeyPair",
                "lightsail:GetInstanceAccessDetails",
                "lightsail:GetKeyPair",
                "lightsail:GetKeyPairs",
                "redshift:GetClusterCredentials",
                "s3:GetBucketPolicy",
                "storagegateway:DescribeChapCredentials"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

### Using the AWS Chatbot Lambda\-Invoke policy<a name="about-lambda-invoke-chatbot-policy"></a>

The AWS Chatbot **Lambda\-Invoke Command Permissions** policy allows users to invoke AWS Lambda functions in chat channels\. This policy is an AWS managed policy that is not specific to AWS Chatbot, though it appears in the AWS Chatbot console\.

By default, invoked Lambda functions can perform *any operation*\. You might need to define a more restrictive inline IAM policy that allows permissions to invoke specific Lambda functions, such as functions specifically developed for your DevOps team that only they should be able to invoke, and deny permissions to invoke Lambda functions for any other purpose\.

The following example shows the **Lambda\-Invoke Command Permissions** policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "lambda:invokeAsync",
                "lambda:invokeFunction"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

You can also define resource\-based permissions to allow invoking of Lambda functions only against specific resources, instead of the "\*" wildcard that applies the policy to all resources\. Always follow the IAM practice of granting only the permissions required for your users to do their jobs\.

## Protection policy<a name="cbt-protection-policy"></a>

The expansion of usable CLI commands occurred on 11/28/2021\. This expansion can allow channel members to create, read, update, and delete your AWS resources\. To prevent this, a protection policy is applied as a guardrail policy to existing AWS Chatbot configurations by default\. Specifically, the protection policy restricts permissions and actions to what was available before all CLI commands were usable\. This policy is detachable, but we strongly recommend it stay in place until you’ve verified that all your guardrails, channel IAM roles, and user\-level roles align with your governance policy or channel requirements\. You can detach this policy from:
+ Individual workspaces\.
+ Individual channels in the channel configurations page\.
+ A selection of channels using the **Set guardrails** button\.
+ All channel configurations in the **User permissions** page of the AWS Chatbot console\.

The following example shows the protection policy JSON code:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "a4b:Get*",
                "a4b:List*",
                "a4b:Describe*",
                "a4b:Search*",
                "acm:Describe*",
                "acm:Get*",
                "acm:List*",
                "acm-pca:Describe*",
                "acm-pca:Get*",
                "acm-pca:List*",
                "amplify:GetApp",
                "amplify:GetBranch",
                "amplify:GetJob",
                "amplify:GetDomainAssociation",
                "amplify:ListApps",
                "amplify:ListBranches",
                "amplify:ListDomainAssociations",
                "amplify:ListJobs",
                (...)
                "xray:BatchGet*",
                "xray:Get*",
                "lambda:Invoke*",
                "support:*",
                "ssm-incidents:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```