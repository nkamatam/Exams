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
	Makes is easy to setup, operate and send notifications from the cloud. It provides developers with highly scalable, flexible and cost effective capability to puclish messages from an application and immediately deliver to the subscribers or other applications. 
* Push notifications to Android, iOS, FireOS and Windows mobile devices and to Android devices in China using 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMDE3NjU1NzMsLTE4Mjk5NzYzNTIsMj
A4MzkzNjU2LDczMDk5ODExNl19
-->