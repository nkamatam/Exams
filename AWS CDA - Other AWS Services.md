## SQS

* Example of Travel Website
* What is SQS
		* Using Amazon SQS, you can decouple the components of an application so that they run independently, easing message management between components.
		* Any component of a distributed application can store messages in the queue. Messages can contain upto **256 KB** of text in any format. Any compontnt can later retrieve the messages programatically using the Amazon SQS API.
		* The queue acts as a buffer between the component producing and saving the data, and the component recieving the data for processing. This means the queue resolves issues that arise if the producer is produving work faster than the consumer can process it, or if the producer or consumer are only intermittently connected to the network.
		* Cool Down and Spike situations have to be handled separately using auto scaling.
* Standard Queues:
* Unlimited number of transactions per second. They guarentee that the message is delivered at least once. However, occasionally (because of the highly-distributed architecture that allow high throughput), more than one copy of a message might be delivered out of order. Standard queues provide best-effort ordering which ensured that messages are generally delivered in
		* 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjExODg3NTAxMSw3MzA5OTgxMTZdfQ==
-->