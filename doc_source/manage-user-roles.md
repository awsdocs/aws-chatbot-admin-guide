# Managing user roles<a name="manage-user-roles"></a>

All users can manage user roles:
+ Channel members can switch their user roles from their chat channels\. Additionally, channel members can unmap user roles from chat client IDs using the AWS Chatbot console\.
+ Administrators can unmap user roles from channel members' chat client IDs from the **User permissions** page in the AWS Chatbot console\. Administrators can also require user roles by enabling a user role requirement in the **User permissions** page\. This requirement can be applied to all workspaces and channels or to individual channel configurations\. For more information on user role requirements, see [User role requirement](understanding-permissions.md#role-reqs)\.

**Note**  
Administrators can't map user roles\. Only channel members have this ability\.

**Topics**
+ [Prerequisites](#ur-prereq)
+ [Channel members: Adding a user role from a chat channel](#cm-add-role)
+ [Channel members: Switching user roles from a chat channel](#cm-switch-role)
+ [Channel members: Unmapping a user role](#cm-unmap-role)
+ [Administrator: Unmapping a user role](#admin-unmap-role)
+ [Administrator: Enabling a user role requirement](#admin-ur-req)

## Prerequisites<a name="ur-prereq"></a>

To manage user roles, you need a chat channel configured for AWS Chatbot\. For more information, see [Getting started with AWS Chatbot](getting-started.md)

## Channel members: Adding a user role from a chat channel<a name="cm-add-role"></a>

If you are a new channel member or your channel permission approach changes, AWS Chatbot will prompt you to add a user role\.

**To add a user role from a chat channel**

1. Choose **Let's get started**\.

1. Choose an account to add a role\.
**Note**  
This link will take you directly to the AWS Chatbot console\.

1. In **User role**, choose a role\.

1. Choose **Save**\.
**Note**  
 Choosing **Save** takes you to an authorization page to fetch your chat client identity\. This identity is mapped to your chosen role\.

1. Choose **Allow**\.

## Channel members: Switching user roles from a chat channel<a name="cm-switch-role"></a>

If you find that your current user role doesn’t have the right permissions to achieve your desired task, you can switch roles directly from Microsoft Teams and Slack\.

**Note**  
If you are unable to run a particular command after switching roles, contact your administrator regarding the channel guardrails in place\.

**To switch a user role from a chat channel**

1. In your chat channel, enter `@aws switch-role`\.

1. Choose the account that you want to switch roles for\.
**Note**  
This link will take you directly to the AWS Chatbot console\.

1. In the AWS Chatbot console, choose **Choose user role**\.

1. In **User role**, choose a user role\.

1. Choose **Save**\.
**Note**  
Choosing **Save**, takes you to an authorization page\. This is so your chat client identity can be retrieved and associated with your chosen role\.

1. On the authorization page, choose **Allow**\.

## Channel members: Unmapping a user role<a name="cm-unmap-role"></a>

If you have a user role applied that you no longer need, you can unmap it\.

**To unmap a user role**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Choose a configured client\.

1. In **User role**, choose **Clear role**\.

## Administrator: Unmapping a user role<a name="admin-unmap-role"></a>

You can unmap a user role from a chat client ID\. When you unmap a user role, it will no longer appear your **Mapped roles** table\.

**To unmap a user role**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Under **Account settings**, choose **User permissions**\.

1. In **Mapped roles**, select the roles you want to unmap\.

1. Choose **Unmap**\.

## Administrator: Enabling a user role requirement<a name="admin-ur-req"></a>

You can enable a user role requirement to force users to apply a user role before running commands in Microsoft Teams and Slack\.

**To enable a user role requirement**

1. Open the [AWS Chatbot console](https://console.aws.amazon.com/chatbot/)\.

1. Under **Account settings**, choose **User permissions**\.

1. In **User role requirement**, enable a user role requirement\.