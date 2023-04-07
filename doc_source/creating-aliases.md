# Creating and using command aliases in chat channels<a name="creating-aliases"></a>

AWS Chatbot supports the creation of command aliases to make running commonly used CLI commands easier\. A command alias is a custom shortcut or shorthand representation of a CLI command that you define\. A command alias can be configured to include one or more custom parameters\. Additional target actions can be included as part of the alias definition\. 

Command aliases are defined for a single channel\. If a channel is configured to work with more than one AWS account, the aliases work across all AWS accounts\. All channel members share the aliases defined in the channel\.

**Note**  
When a channel member runs a command alias, the target is run with the configured member's permissions \(channel role, user role\) depending on the permissions schemas in place\. For more information about permissions, see [Understanding AWS Chatbot permissions](understanding-permissions.md)\.

We strongly recommend that you don't add any personally identifiable information \(PII\) or other confidential or sensitive information to your command aliases\.

For more information about running CLI commands in Slack, see [Running AWS CLI commands from chat channels](chatbot-cli-commands.md)\.

**Topics**
+ [Creating a command alias](#create-aliases)
+ [Listing command aliases](#list-aliases)
+ [Running command aliases](#run-alias)
+ [Getting help](#get-help)

## Creating a command alias<a name="create-aliases"></a>

To create a command alias, run:

`@aws alias create alias_name mapped_action`\.

**Note**  
The command alias definition can contain additional parameters\. Also note that *alias\_name* has a 100 character limit and *mapped\_action* has a 5,000 character limit\.

For example, in this alias:

`@aws alias create automation ssm start-automation-execution --document-name $name --parameters { "AutomationAssumeRole": ["arn:aws:iam::123456789012:role/$role-name"] }`

The parameters `$name` and `$role-name` are input variables that are required to run the alias\. When the alias is run, your supplied parameter values are used for the parameters\.

So, if you run:

`@aws run automation val1 val2`\.

`$name` is assigned the value val1 and `$role-name` the value of val2\. These values are assigned by position\.

Alternatively, you can explicitly name the alias parameters:

`@aws run automation --name val1 --role-name val2`\.

## Listing command aliases<a name="list-aliases"></a>

 To list all available command aliases in a channel, run:

` @aws alias list`\.

## Running command aliases<a name="run-alias"></a>

To run a command alias, use:

`@aws run alias_name` or `@aws alias run alias_name`\.

## Getting help<a name="get-help"></a>

To get help with command aliases, run:

`@aws help` or `@aws alias help`\.