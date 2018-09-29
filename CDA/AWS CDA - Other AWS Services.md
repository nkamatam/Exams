## SQS

* Example of Travel Website
* What is SQS
		* Using Amazon SQS, you can decouple the components of an application so that they run independently, easing message management between components.
		* Any component of a distributed application can store messages in the queue. Messages can contain upto **256 KB** of text in any format. Any compontnt can later retrieve the messages programatically using the Amazon SQS API.
		* The queue acts as a buffer between the component producing and saving the data, and the component recieving the data for processing. This means the queue resolves issues that arise if the producer is produving work faster than the consumer can process it, or if the producer or consumer are only intermittently connected to the network.
		* Cool Down and Spike situations have to be handled separately using auto scaling.
* Standard Queues:
	* Unlimited number of transactions per second. They guarentee that the message is delivered at least once. However, occasionally (because of the highly-distributed architecture that allow high throughput), _more than one copy_ of a message might be delivered out of order. Standard queues provide **best-effort ordering** which ensured that messages are **generally delivered** in the same order as thet are sent.
* FIFO Queues:
	* The FIFO queue complements the standard queue. The most important features of this queue type are FIFO delivery and exactly once processing: The order in which messages are sent and recieved is strictly preserved and a message is delivered once and remains available until consumer procesess and deletes it; duplicates are not introduced into the queue. FIFO queues also suport message groups that allow multiple ordered message groups within a single queue. FIFO queues are limited to 300 transactions per second(TPS), but have all the capabilities of standard queues.
	
	## SNS
	Makes is easy to setup, operate and send notifications from the cloud. It provides developers with highly scalable, flexible and cost effective capability to publish messages from an application and immediately deliver to the subscribers or other applications. 
* Push notifications to Android, iOS, Fire OS and Windows mobile devices and to Android devices in China using Baidu Cloud Push.
* Besides pushing cloud notifications directly to mobile devices, Amazon SNS can also deliver notifications by SMS text messages or email to SQS or to any HTTP endpoint.
* SNS notifications can also trigger lambda functions: When a message to delivered to a SNS topic that has a Lambda subscribed to it, Lambda is invoked when with the message as the payload. When the Lambda function receives the message as the payload, it can manipulate the message and publish to another SNS topic or to any other AWS service.
* **SNS Topic**
* SNS allows you to to group multiple recipients using Topics. A Topic is an "access point" for the subscribers to dynamically subscribe to identical copies of the same notification.
* One topic can support deliveries to multiple end-point types - for example you can group together iOS, Android and SMS recipients. When you publish once to a Topic, SNS delivers appropriately formatted copies of your message to each subscriber.
* **Availability** : To prevent the loss of a message, all the SNS topics and messages are stored redundantly across AZs.
### SNS Benefits
* Delivery is instantaneous and _push based_(no polling)
* Simple APIs and easy integration with Applications
* Flexible message delivery over multiple transport protocols
	* SQS
	* HTTP
	* SMS
	* Push Notifications to multiple mobile devices
* Inexpensive Pay-as-you-go model
* Web Based AWS console offers easy point-and-click interface

### SNS vs SQS
* Both are messaging services
* SQS - Pull, SNS - Push
### SNS Pricing
* $0.06 per 10,000 Notification Deliveries over HTTP
* $0.75 per 100 notification deliveries via SMS
* $2 per 100,000 notification deliveries via email

## SNS 
* SNS follows the pub-sub paradigm, with notifications being delivered to the clients with a push mechanism that eliminates the need to periodically check for new information or updates
* * With simple APIs requiring minimal development effort, no maintenance and management overhead and pay-as-you-grow pricing, SNS gives developers an easy mechanism to incorporate a powerful notification system with their applications.

### SNS Exam Tips
* SNS is scalable and highly available notification service which allows you to push notifications from the cloud.
* Veriety of message formats supported : SMS, email, SQS and HTTP endpoint
* pub-sub model where users subscribe to Topics
* It is a push mechanism than a pull (poll) mechanism

## SES vs SNS

### SES
* SES is a scalable and highly available email service designed to help marketing teams and application developers to send marketing, notification and transaction emails to their customers using pay-as-you-go model.
* Automated emails:
	* Ex: Automated emails to customers
* Can also be used to receive emails - incoming emails can be delivered to an S3 bucket
* Incoming emails can be used to trigger SNS notifications or Lambda functions

|SES|SNS|
|--|--|
|Email Messaging Service|Pub/Sub model messaging service (SMS, HTTP, SQS, email)|
|Can trigger Lambda function or SNS notification| Can be used to trigger LAmbda function|
|Can be used for both incoming and outgoing email| Can fan out emails to large number of recipients|
|An email address is all that is needed to send email to the users|Consumers must subscribe to a topic to


 

* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzM4MjgwMjIwLDkzNjA5NjEwM119
-->