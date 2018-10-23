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
|Can be used for both incoming and outgoing email| Can fan out emails to large number of recipients (replicate messages to multiple formats and endpoints)|
|An email address is all that is needed to send email to the users|Consumers must subscribe to a topic to get notifications|

### SNS vs SES Exam Topics
* See the above table

## Kinesis - 101

### What is streaming Data
* Generated by thousands of data sources
* Sent as data records 
* Countinous and in small KBs of data
	* Purchase transaction on retail store
	* Stock prices
	* Game Data (Angry Birds etc)
	* Social Network Data (FB Posts etc)
	* Geo Spacial Data (Ex: Uber - Constantly tracking Uber driverf's location and give you a fastest way to reach the customer)
* IoT Data
### What is Kinesis
* Platform to send your straming data to.
* Easy to load and analyse straming data
* Ability to build your own custom application for your business needs
### Kinesis Services
* Streams/Girehose/Analytics
![KS](https://github.com/nkamatam/Exams/blob/master/CDA/KStreams.png)

### K-Streams

Producers ---> [ Kinesis Streams ]
					      [Shrads                   ]   -----> EC2 instances

* Consists of Shrads
* 5 transactions / second
* up to a max total data read tate of 2 MB / Sec
* up to 1,000 records per second for writes
* up to a maximum total data write rate of 1 MB / second (including partition keys)
* The data capacity of your sctream is a function fo teh number of Shrads that you specify for the stream. The total capacity of the stream is the sum of the capabilities of its shrads
### Kinesis Firehose
* Producers are sending the data to Kinesis firehose ( no shrads and streams, no manual addition of shrads. It is completely automated), You do not need to concern yourlsef about data consumers mining the data from Firehose. It can analyse the data using Lambda in real time. The anaytics  can be performed on this data and that can be sent to S3. The anaylytics on the data is completely optional.
![](https://github.com/nkamatam/Exams/blob/master/CDA/FieHose.png)


* There is no automatic data retention window in Amazon Firehose. In Kinesis Streams, there is such a window (24 hours by default which can be extended to 7 days) but in Firehose there is none. It is either analysed using lambda or directly sent to S3 or (Redshift (wite to S3 first and then copied to Redshift), Elastic Search cluster ( can directly write). Firehose is a very automatic way of doing this. No need to worry about shrads etc

Kinesis Analytics
* Allows you to run sql queries on the data coming from Kinenis streams/fh. Way of analysis data using SQL type landuages across (Kinesis streams/Firehose)

#### Exam Tips
* Understand the difference between Kinesis Streams and Kinesis Firehose. Will be needed to find what service is used when. Ex: Shrads/Analysing data using Lambda
* Understanding what is Kinesis Analytics.

### Kinesis Lab
* Will setup Kinesis Streams using Cloud Formation
* Change the region to North Vergenia
* Use the cloud formation - The template is present in the resources section of the course.


### Elastic Beanstyalk 101
* What is Elastic Beanstalk
	* It is a service that helps deploying and scaling web applications developed in many popular languages : Java, .NET, PHP, Node.js, Python, Ruby, Go and Docker onto widely used server platforms like Apache Tomcat, Nginx, Passenger and IIS. 
	* Developers can focus on writing code and don't need to worry about any of the underlying infrastructure that is needed to run the applications.
	* Throw the code in a ZIP file and Elastic Beanstalk will detect and configure the environment needed to deploy the application, Ex: Webservice, Autoscaling, RDS (or you can manage yourself). Similar to cloud formation (but CF is based on JSON) but Elastic Beanstalk will delpoy all for you.

#### What is Elastic Beanstalk?
* Fastest and the simplest way to deploy your code to AWS
* Automatically scales your applications up or down
* You can select the suitable EC2 instance type for your needs.
* You can retain full admin control of all the infrastructure created by Elastic Beanstalk or have it do for you
* Managed platform updates will automatically update your operating systems, Java, PHP, Node.js etc
* Monitor and Manage the health via dashboard
* Integrated with Cloud Watch and X-Ray (for performance data and metrics)

#### Elastic Beanstalk Exam Tips
* Fastest and the simplest way to deploy your code to AWS
* Automatically scales your applications up or down
* You can select the suitable EC2 instance type for your needs.
* You can retain full admin control of all the infrastructure created by Elastic Beanstalk or have it do for you
* Managed platform updates will automatically update your operating systems, Java, PHP, Node.js etc
* Monitor and Manage the health via dashboard
* Integrated with Cloud Watch and X-Ray (for performance data and metrics)

### Deploy Applications with Elastic Beanstalk

* Steps:
* Resources : Version 1
* Goto Elastic Beanstalk
* Instead sample app, please chose the local file and upload it.
* Chose the environment to the PHP - and Choose "Create Application"
* You can goto the website link and see that the application is deployed well.
* You can go and configure the environment created by Elastic Beanstalk (ex: Server type etc)
* "Rolling updates and deployments" is an important exam topic and will be covered in the next lecture
* "Mamaged Updates" are for updates for the underlyng platform (ex. PHP etc)

### EBS Deplpoyment Policy
* Elastic Beasntalk provides several options for processing deployments:
	* All at once
	* Rolling
	* Rolling with additional batch
	* Immutable

#### All at once deployment policy
* It updates
	* Deploys the new versions to all instances simultaneously
	* All iof your instances are out of service while the deployment take place
	* You will experience an outage while the deployment is taking place - not ideal for mission critical production systems
	* If the update fails, you need to roll back the changes by re-deploying the original version to all your instances
#### Rolling updates policy
* Deplys the new version in batches
* Each batch of instances is taken out of servi ce while the deployment takes place
* Your environment capacity will be reduces by the number of instances in a batch while the deployment takes place
* Not ideal for performance sensitive applications
* If the update fails, you need to perform an additional rolling update to roll back the changes

#### Rolling with Additional Batch Policy
* Launches an additional batch of instances
* Deploys the new version in batches
* Maintains full capacity during the deployment process
* If the update fails, you need to perform an additional rolling update to roll back the changes
#### Immutable Deploymenrt Policy
* Deploys the enw version to a fresh group of instances in theor own new autoscaling greoup
* When the new instances pass their health checks, they are moved to your existing auto scaling group; and finally the old instances are terminated.
* Maintains full capacity during the deployment policy
* 



 

* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNTA0MTUwNjQ1LC0zNzU1MDE3NDYsODQ2MT
E1NTU0LDEzNTk0NTQxNjMsOTAwOTY0NDI2LC0xOTI0MzE5MzA3
LC0xNDQyMDg3ODAyLC00OTk5MDExNzYsLTEyNjg1MzMwMzQsMT
Q0MDA3MDMyNiwxMzU1ODI0MjEwLC0xNjQ5Nzc3NTY2LDkzNjA5
NjEwM119
-->