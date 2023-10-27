# SNS2IFTTT

An [AWS Lambda](https://aws.amazon.com/lambda/) function to push [Amazon SNS](https://aws.amazon.com/sns/) notifications to [IFTTT](https://ifttt.com) via the [Maker Webhooks](https://ifttt.com/maker_webhooks) service.

Code forked from https://github.com/twaluigi/sns2ifttt.  Updated for Node.js 18.x runtimes.

## Installation

### On IFTTT

1. Go to [https://ifttt.com/maker_webhooks](https://ifttt.com/maker_webhooks) and write down your secret key

### On AWS

2. Create a new Amazon SNS topic, e.g. `sns_to_ifttt`
3. Create a new AWS Lambda function, e.g. `sns-to-ifttt`
   1. Use Node.js as runtime
   2. Paste the code inline from the `index.js` file included in this repository
   3. Set the environment variable `iftttMakerEventName` to be the name of your IFTTT webhook `event_name` (environment variables are set in the Configuration tab in the web interface)
   4. Set the environment variable `iftttMakerSecretKey` to be your maker `key`
   5. Leave the default handler
   6. Use a basic execution role
   7. Leave the default memory (128MB) and timeout (3s)
4. Add SNS as an event source to the Lambda function
   1. Choose the SNS topic created at step 1
   2. In the options, enable the event source now (not later)

### On IFTTT

5. Select My Applets
6. Create an Applet
7. Choose Webhooks ('this')
8. Select Receive a Web Request
9. Write the Event Name exacly as is the `iftttMakerEventName` variable of the Lambda function (step 3.3 on AWS)
10. Select Create Trigger
11. `Value1` contains the Subject of the SNS message
12. `Value2` contains the Message body of the SNS message
13. Choose whatever you want as Action ('that'), for example:
   1. iOS or Android Notifications to receive it on your mobile (you need the IFTTT app installed on the device), e.g. you can  set the notification to `SNS {{Value1}} {{Value2}}`
   2. Slack to send a message to your team
   3. Trello to create a new card
   4. GitHub to create a new issue

### On AWS (optional)

14. You can test the setup from the SNS web console
   1. Select the topic
   2. Publish a test message
