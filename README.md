# Assignment-2.14
SQS/SNS/Eventbridge

1.Does SNS guarantee exactly once delivery to subscribers?

    No, Amazon SNS (Simple Notification Service) does not guarantee exactly-once delivery.

Here's what SNS guarantees:

At-least-once delivery: SNS will make a best effort to deliver each message to every subscribed endpoint at least once.

No ordering guarantees: For most protocols (like HTTP/S, Lambda, SQS), SNS does not guarantee message ordering.

Potential for duplicates: Under certain conditions (like network retries or subscriber timeouts), SNS may deliver the same message more than once.

2.What is the purpose of the Dead-letter Queue (DLQ)? This is a feature available to SQS/SNS/EventBridge?

A DLQ (Dead-Letter Queue) is a special type of message queue that holds messages that cannot be processed or delivered successfully due to various reasons, such as errors, expired messages, or queue capacity limits. Its main purpose is to prevent the source queue from overflowing and to provide a place to store problematic messages for later analysis, debugging, or redelivery.

SQS: A DLQ is a separate SQS queue that is configured to receive messages from a source queue. When a message cannot be delivered, it's sent to the DLQ. 
SNS: An SNS subscription can be configured to send undeliverable messages to a DLQ (which is an SQS queue). 
EventBridge: EventBridge can also use DLQs to capture events that fail to be delivered to their intended targets. 


3.How would you enable a notification to your email when messages are added to the DLQ?

To enable email notifications when messages are added to a Dead Letter Queue (DLQ) in AWS, you can follow this general approach:

1. Create an SNS Topic
This topic will send email notifications.

2. Subscribe Your Email to the SNS Topic
    Choose "Email" as the protocol and provide your email address.

    Confirm the subscription via the confirmation email.

3. Create an Amazon CloudWatch Alarm
    Use CloudWatch metrics to monitor the ApproximateNumberOfMessagesVisible on the DLQ (i.e., the number of messages in the DLQ ready to be read).

    Configure an alarm to trigger when the metric is greater than zero (or your desired threshold).

4. Set SNS Topic as the Alarm Action

    When the alarm state is ALARM, it will send a notification to the SNS topic, which in turn will notify your email.

