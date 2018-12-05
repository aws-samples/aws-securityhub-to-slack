# aws-securityhub-to-slack
Demonstrates sending AWS Security Hub findings to your Slack WorkSpace 

## Coming Soon!
This is the companion GitHub repository for the AWS blog post ... x.x.x.x.x.x.x.x.x.x.x

This repo will introduce you to the process of creating AWS Security Hub a custom action by sending findings to Slack.  After reading this blog you will understand the process to create your own custom actions for utilization in your Security Operations play books.

## Send to Slack Custom Action

1.	**Prerequisites**
    + AWS Security Hub is enabled
    + Membership in a Slack workspace (https://get.slack.help/hc/en-us/articles/212675257-Join-a-Slack-workspace)
2.  **Create an incoming Webhook in Slack API**
    + Go to your Slack API web page to create the Webhook (https://api.slack.com/incoming-webhooks#create_a_webhook)
    + Click on Create Your Slack App button
    + Click on Create New App button  
    
      **App Name**: "SecurityHubToSlack"  
      **Development Slack Workspace** : "Choose the Slack workspace that will receive the Security Hub Findings"          
 
    + Click on the **Create App** Button
    + Select **Incoming Webhooks** 
    + At the “Activate Incoming Webhooks” Screen
    + Move the slider from OFF   to ON  
    + Scroll down and **Add New Webhook to Workspace**
    + Select the Slack Channel in your Slack Workspace that the Security Hub findings will be posted to and select Authorize (suggestion “#alerts”)
    + On the next screen, scroll down to the Webhook URL section and click the **Copy** button, so we can use it as input in our CloudFormation template
 
3.	**Launch Cloud Formation Template** . 
This CloudFormation template will create a Lambda Function that utilizes Slack’s Webhook API feature, as well as a CloudWatch Event Rule to send findings from Security Hub’s custom actions to Slack.
    + Download CloudFormation template by right clicking on “SecurityHubFindingsToSlack.json” and “Save Link As..” on your local machine
    + Navigate to https://console.aws.amazon.com/cloudformation/
    + Select Create stack
    + Select Upload a template file
    + Select Choose file and locate “SecurityHubFindingsToSlack.json” on your local machine
    + Select Next
    + Use the following values to fill out *Create Stack* parameters  
    
   **StackName**: EnableSecurityHubFindingsToSlack  
      **IncomingWebHookURL**: Paste URL that you just copied from Slack API pages  
      **SlackChannel**: Enter the same Slack Channel name that you chose above (#alerts)  
      **MinSeverityLevel**: Choose the minimum Severity Level you want to be notified in Slack, example HIGH would only send high severity findings, LOW sends all findings   

   + Complete Create Stack form
   + Select Next, fill out any Tags and select Next again
   + Accept IAM Resource creation
   + Select Create Stack, CloudFormation will then begin creating the stack
   + Wait for the CloudFormation console to report stack creation complete

4.	**Create Security Hub Custom Actions** . 
    + In the Security Hub navigation pane (https://console.aws.amazon.com/securityhub/) select Settings then choose the **Custom Actions** tab. 
    + Select **Create custom action**. 
    + Then in the Create custom action pop up, specify the action name, description and ID then choose OK to create the action.
    
    **Use these values to fill Create Custom Action parameters**  
    
      **Name**: Send to Slack  
       **Description**: This custom action sends selected findings as channel in a Slack Workspace 
       **Custom action ID**: SendToSlack  

5.	**Testing the Send to Slack Custom Action**
    + Navigate to AWS Security Hub Console (https://console.aws.amazon.com/securityhub/)
    + Navigate to Findings
    + Select the check box next to one or more findings
    + Click the drop-down Actions menu and choose the **Send To Slack** Custom Action

The Security Hub Console will then send the finding to your Slack channel, you should then receive a notification in your Slack channel 
