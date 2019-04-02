# Simple Queue Service (SQS)
### What is it?
* Service that gives you access to a message queue that can be used to store messages that are awaiting processing
* A pull-base system (pulls messages(jobs) down)
* Using SQS, you can decouple the components of an app so they run independently for easier message management
* Messages can contain up tp 256 KB of text in any format
* messages can be kept in the queue from 1 minute to 14 days, default retention period is 4 days

### Two types of queues
*Standard Queue (default)*
* allows nearly unlimited number of transactions per second
* guarantees that a message is delivered at least once
* duplicates may occur
* best-effort ordering to ensure messages are generally delivered in the same order as they are sent

*FIFO*
* order in which messages are sent and received is strictly preserved
* a message is delivered once and remains available until a consumer processes and deletes it
* duplicates are not introduced into queue
* support message groups that allow multiple ordered message groups within a single queue
* limited to 300 transactions per second

### Visibility timeout
* the amount of time that the message is invisible in the SQS queue after a reader picks up that message
* if job is processed before visibility timeout expires, message is deleted from queue
* if job is not processed within that time, message will become visible again and another reader will process it
  * may result in message delivered twice
* Default is 30 seconds
  * Should increase this if your task takes >30 seconds
* Max is 12 hours

### Long Polling
* A way to retrieve messages from SQS queues
* Does not return a response until a message arrives in queue, or until long poll times out

Normally, with regular short polling (default), will continue pulling (asking if there's a job) even when message queue is empty


# Simple Notification Service
### What is it?
* A service that makes it easy to set up, operate, and send notifications from the cloud
* Can send push notifications to Apple, Google, Fire OS, and Windows mobile devices, and Android devices in China with Baidu Cloud Push
* Can also send notifications by SMS text or email to SQS queues, or to any HTTP endpoint
* Can trigger Lambda functions - lambda function receives the message payload as an input parameter and can manipulate the info in the message, publish the message to another SNS topic, or send message to other AWS services
* To prevent messages from being lost, all messages published to SNS are stored redundantly across multiple availability zones
* push-based
* publish-subscribe" model - users subscribe to topics

### Topics
* SNS allows you to group multiple recipients using topics (can support deliveries to multiple end points)

### Benefits
* instantaneous, pushed-based delivery (no polling)
* simple APIs and easy integration with applications
* flexible message delivery over multiple transport protocols
* inexpensive pay-as-you-go model

### Pricing
* $0.50 per 1 million SNS Requests
* $0.06 per 100,000 notification deliveries over HTTP
* $0.75 per 100 notification deliveries over SMS
* $2.00 per 100,000 notification deliveries over email
