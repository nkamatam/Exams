### DynamoDB
* Fast/Flexible/NoSQL Database
* Single Digit milli-second latency at any scale
* Fully managed by AWS
* Supports
	* Document
	* Key-Value Model
* Flexible data model
* Good for 
	* Gaming/Mobile/Web/Ad-Tech/IoT
* Stored on SSD
* Spread across 3 geographically distinct locations
* Choice of 2 consistency models
	* Eventual Consistent Reads (Default)
	* Strongly Consistent Reads

### Creating a Dynamodb Table - Lab
* Create a new Role on IAM
* Create Role
* Service Role for EC2
* Policy Type - Dynamodb Full Access
* Name the role as XXX
* Goto Services and goto EC2
* Select AMZLinux - t2.micro
* IAM role (xxxx)
* Boot Strap script - Paste it from material
	* install apache/php and git
	* create a php small script
	* Gets the files from github
* HTTP and SSH - Create a SG accordingly
* Launch the instance
* Make sure you use a key to login to that server
* Login to that server ssh -i xxxx
* Install composer from the command prompt
* Change the region in the create tables script under dynamodb directory
* Goto the Dynamodb on console
* Invoke the createtables.php, uploaddata.php from the website
* Look at the data in the dynamodb tables
* Notice that not all items have the same number of attributes
* You can query/scan based on a product id from the console
* **Interacting with Dynamodb from CLI**: 
	* EC2 should be able to do it because of the role that we created
	* aws dynamodb get-item --table-name ProductCatalog --key '{"Id":{"N":"205"}}
	* Go back to the IAM and goto the xxxx role and delete it.
	* Now, you will realise that you are not able to run the same command again

### Indexes on Dynamodb
* **What is an Index**
	* In SQL databases, an Index is a data structure which allows you to perform fast queries on specific columns of the table. You select the columns that you need to be queried on and include them in the index - and run your searches on that index rather than the entire dataset.
	* In Dynamodb, there are two types of indexes
		* Local Secondary Index
		* Global Secondary Index
* Local Secondary Index:
	* Can only be created when you are creating the table
	* You cannot add/remove or modify it later
	* It has the same partition key as your original table
	* But, it has a different Sort Key
	* Gives you a different view of your data, organised according to an alternate Sort Key
	* Any queries based on this Sort Key are much faster using the index than the main table
		* eg: Partition Key  = User Id
		* Sort Key = Account Creation Date
* Global Secondary Index
	* You can create it when you create the table or add it later
	* Different partition key as well as different Sort Key
	* So, it gives you a completely different view of the data
	* Speeds up any queries related to this Partition Key and the Sort Key
		* Eg: Partition Key - email id
		* Sort Key : Last Post date
* **Dynamodb Indexes - Exam Tips**
	* Indexes allow fast queries on specific columns
	* Gives you a different view of the data based on the PartitionKey and the SortKey
	* 
|**Local**|**Global**|
|--|--|
| Must be created when the table is created | Can create at any time - Creation time or later |
|Same partition Key as your table|Different Partition Key|
|Different Sort Key|Different Sort Key|

### Scan vs Query API
* **What is a query**
	* A query operation on a table finds the items in a table based on a Primary Key attribute and a distinct value being searched for
	*eg: Select an item where user id = 212, will select all attributes of that item; ex: First Name, Email address, Surname etc
	* You can use an optional Sort Key (and Value) to refine the results
	* Eg: If the sory key is a time stamp, you can refine the query to only select the items that are in the last 7 days
	* By default, a Query returns all the attributes for the items but you can use the **ProjectionExpression** parameter if you want the query to only return the specific attributes that you want.
	* Eg: if you only want to see the email address rather than all the attributes
	* Results are always sorted by the sort key
	* Numeric order - By default is in ASC order
	* ASCII character code values
	* You can reverse the order by setting the **ScanIndexForward** parameter to _false_
		* **This only applies to queries - and NOT SCANS**
	* By default, the Queries are eventually consistent
	* You need to explicitly set the query to be strongly consistent
* **What is a Scan?**
	* A scan operation examines every item in the table
	* By default returns all attributes
	* Use **ProjectExpression** parameter to refine scan to only return the attributes you want
	* Scan will dump the whole data and then fliter based on the filter condition
* **Query vs Scan**
	* Query is more efficient than Scan - 
		* Scan is a full table scan followed by filter
		* Scan takes longer takes longer and longer
		* Can be expensive
* **Improving the perfromance**
	* You can reduce the impact of a Query/Scan by setting a smaller Page Size which uses fewer read operations
	* Eg: Set the page size to return 40 items
	* Larger number of smaller operations will allow other requests to succeed without getiing  throttled
	* Avoid using Scan opearations if you can, design the tables in a way that you can use the Query, Get or BatchGetItem APIs.
	* **Scan Performance:**
	* By default, a scan operation operates sequentiall in returning the data in 1MB increments before moving on to get the next 1MB data. _It can only scan 1 partition at a time_
	* You can configure Dynamodb to user Parallel Scans instead by logically deviding a table or index into segments and scanning each segment of the data in parallel.
	* Best to avoid parallel scans if your table or index is already incurring heavy read /write activity from other applications.

**Scan vs Query**
* A query operation finds items in a table using only the Primary Key attribute
* You provide Primary Key name and a distinct value to search for
* A Scan operation examines every item in the table
* By default returns every item in the table
* By default returns all data attributes
* Query results are always sorted by the sort key
* Sorted in ASC order
* Set ScanIndexForward parameter to _false_ to reverse the order - for queries only
* Query operation is in-general more efficient than a Scan operation
* Reduce the impact of a query or a scan by setting a smaller page size which uses fewer read operations
* Isolate scan operations to specific tables and segretgate them from your mission critical traffic
* Try parallel scans, rather than default sequential scan
* Avoid using scan operations if you can: design tables in a way that you can use the Query, Get or BatchItem APIs

### Dynamodb Provisioned Throughput**
**DynamoDB read and write capacity units**
* DDB Provisioned Thoughput is measured in  capacity units
* When you create a table, you mention the requirements in terms of READ and WRITE capacity units (unless you specify AUTO SCALING)
* 1 x  WRITE CU = 1 x 1 KB Write/Sec
* 1 x READ CU = 
	* 1 x Strong Consistent Read of 4KB/Sec 
<br>(or) <br>
2 x Eventual Consistent Reads of 4 KB/Sec (Default)
* DynamoDB Example Configuration
	* Table with 5 x RCU and 5 x WRC
	* This configuration will be able to perform
		* 5 x 4KB Strongly Consistent Reads = 20KB/Sec
		* Twice as many Eventually Consistent = 40KB/Sec
		* 5 x 1KB writes = 5 KB/Sec
		* If your application consumes more data, it will consume more Capacity Units and will cost you more as well.
* **Strongly Consistent Reads Calculation**: 
	* Your application needs 80 items/sec.
	* Each item is 3KB in size
	* You need stongly consistenct reads
		* First calculate how many RCUs needed for each Read:
		* Size of each item / 4KB = 0.75
		* Round up to the nearest whole number, each READ will need 1 x RCU per read operation
		* Multiplied by the number of reads/second = 80 RCUs needed
		* 
* **Eventually Consistent Reads Calculation**: 
	* Do the same calculation as above, but **double** the number.
	* Each item is 3KB in size
	* You need stongly consistenct reads
		* First calculate how many RCUs needed for each Read:
		* Size of each item / 4KB = 0.75
		* Round up to the nearest whole number, each READ will need 1 x RCU per read operation
		* Multiplied by the number of reads/second = 80 
		* Divide 80 / 2 = 40 (RCUs for eventually consistent READs

* **Write Capacity Units Calculation**
	* You want to write 100 items/sec
	* Each item is 512 bytes in size
	* First, findout how many capacity units per each WRITE
	* Size of each item /1 KB ( for WCUs)
		* 512 bytes/1KB = 0.5
		* Round to the nearest whole number => 1 x WCU per each write opearation
		* Multiplied with the number of writes (100 items/sec as needed in the problem) = 100 WCU

**DynamoDB Provisioned Throughout - Exam Tips**
* Provisioned Througput is a measure in Capacity Units
* 1 x WCU = 1 x 1 KB Write /Sec
* 1 x RCU = 1 x 4 KB Read/Sec( Strongly Consistent)
*  1 x RCU = 2 x 4 KB Read/Sec( Eventually Consistent)
* Calculation:
	* Calculate how many capacity units per each READ/WRITE
	* => Size of the item / ( 4KB/1 KB)
	* Round it to the nearest whole number.
	* Multiply by the number of reads / sec
	* [Remember for READs : Strongly = 2 x Eventual]

### DAX (DynamoDB Accelerator)

* DAX is a fully managed, clustered in-memory cache for DynamoDB
* Deliveres upto 10 times the read performance improvement
* Micro second performance for millions of requests per second
* Ideal for read-heavy and bursty workloads
* Eg: Auction Applications, Gaming and Retail sites during Black Friday Events
**How does DAX work**
* DAX is a write-through caching service - this means Data is written to the Cache as well as the backend store at the same time
* Allows you to point your DynamoDB API calls at the DAX cluster 
	* Which leads me to believe that DAX cluster is another datalayer over the DynamoDB database        
* If the items that you are querying is in the cache (cache hit), DAX returns the result to the application.
* If the item is not available (cache miss), then DAX performs Eventually Consnstent get-item operation against DynamoDB
* Retrieval of data from DAX reduces the READ load on the DynamoDB database
* May be able to reduce provisioned READ capacity
* **What is it not suitable for**:
	* Caters for Eventually Consistent Reads only - so not suitable for Strong Consistent READS.
	* Write intensive applications
	* Applications that do not perform many READ operations
	* Applications that do not require micro second response times
* **Exam Tips**
	* Provides in-memory caching for DynamoDB tables
	* Improves response times for Eventually consistent Reads only
	* You point your API to the DAX cluster instead of the DynamoDB table
	* If the itesm that you are querying is in the cache(Cache Hit), then DAX will return it otherwise, it will perform eventually consistent read (get-item) operation on your DynamoDB table
	* Not suitable for Write intensive applications or apps that require Strongly Consistent Reads

### Elasticache
* Elasticache - In memory cache in the cloud
* Improves performance of web applications, allowing you to retrieve data from fast inmomory cache rather than slower disk based databases.
* Sits between your aplication and the database
	* Ex: Application requesting data for your popular products
	* Takes the load off your databases
	* Good if your database is particularly read-heavy and the data does not change very often
* Elasticache Benefits and Use Casess
	* Improves performance for read-heavy workloads (eg: Social Networking, Gaming, Media Sharing, Q&A portals)
	* Frequently accessed data is stored locally for low-latency access improving the overall performance of your aplication
	* Also good to compute heavy workloads
		* Ex: Recomendation Engines
* Can be used to store results of I/O intensive database queries ot output of compute-intensive calculations
* 2 Types of Elasticache
	* Memcached
		* Widely adopted memory object caching system
		* Multithreaded
		* No multi-AZ capability
	* Redis
		* Open source in-memory key-value store
		* Support more complex data structures: sorted sets and lists 
		* Supports Master/Slave replication and MultiAZ cross AZ redundancy
		* Better choice if you want higher availability (because of the multi AZ)
* Caching Strategies
	* 2 Strategies available: **LAzy Loading** and **Write-Through:**
	* Lazy Loading - Loading data into the cache only when necessaty
	* If requested data is in the cache, Elasticache retuens the data to the application
	* If the data is not in the cache or has expired, Elasticache returns null
	* Your application then fetches the data from the database and writes the data recieved into the cache so that it is available next time.

**Lazy Loading Advantages and Disadvantages**

|Advantages|Disadvantages  |
|--|--|
|Only requested data is Cached; Avoids filling up the Cache with useless data | Cache miss penalty:Initial request - Query to the database - Writing of the data to the cache|
|Node failures are not fatal. A new empty node will just have a lot of Cache misess initially | Stale data - if data is only updated when there is a cache miss, it can become stale. Doesn't automatically update if the data in the database changes|

**Lazy Loading and TTL**
* Specifies the number of seconds until the key(data) expires to avoid keeping stale data in the cache
* Lazy Loading treats an expired key as a cache miss and causes the application to retrive the data from ther database and subsequently write the data into the cache with a new TTL
* Does not eliminate stale data - but helps to avoid it

**Write-Through Advantages and Disdvantages**
* Write Through - Adds or updates data to the cache whenver data is written to the database

|Disadvantages|Advantages|
|--|--|
|Write penalty: Every write involes a write to the cache as well as a write to the database| Data in the Cache never stale|
|If a node fails and a new one is spun up, data is missing until added ot updated in the datbase(mitigate by implementing Lazy loading in conjunction with write-through)| Users are generally more tolerant of additional latency when updating data than when retrieving it|
Wasted resources if most of the data is never read||

### Elasticache Exam Tips
* In-memory cache sits between your application and database
* 2 different caching strategies: Lazy loading and Write Through
* Lazy Loading only caches the data when it is requested
* Elasticache Node failures not fatal, just lots of cache misses (LL)
* Cache miss penalty: Initial request, query database, writing to cache (LL)
* Avoid stale data by implementing a TTL
* Write Through strategy writes data into the cache whenever there is a cahnge to the database
* Data is never stale
* Write penalty: Each write involves a write to the cache
* Elasticache node failure means that data is missing until added or updated in the databae
* Wasted resouves if most of the data is never used

### **DynamoDB Summary**

* Amazon DynamoDB is a low-latency NoSQL database
* Consists of Tables, Items and Attributes
* Suports both document and key-value data models
* Supported document formats are JSON, HTML, XML
* 2 types of Primary Keys: Partition Key and combination of Partition Key + Sort Key (Composite Key)
* 2 Consistency models: Strong Consistent/Eventually Consistent
* Access is coltrolled using IAM policies
* Fine grained access control using IAM Condition parameter ```dynamodb:LeadingKeys``` to allow users to access only the items where the partition key value matches their user ID.
* Indexes enable fast queries on specific data columns
* Give you a different view of your data based on alternative Partition/Sort Keys
* Important to understand the differences:

|Local Secondary Index|Global Secondary Index|
|--|--|
|Must be created at when you create the table|Can create any time - at the table creation or after|
|Same partition Key as your table| Different Partition Key|
|Different Sort Key| Different Sort Key|

* Scan vs Query
* Query operation finds items in a table using only the Primary Key attribute
* You provide the Primary Key name and a distinct value to search for
* A Scan operation examines every item in the table
* By default, returns all data atributes
* Use the ```ProjectionExpression```parameter to refine the results
* Query results are always sorted by the Sort Key (if there is one)
* Sorted in asceding order
* Set ```ScanIndexForward```parameter to false to reverse the order- this is for Queries only
* Query operation is generally more efficient than a scan
* Performance Improvement
* Reduce the impact of a query or scan by seting a smaller page size which uses fewer read operations
* Isolate scan operations to specific tables and segregate them from your mission-critical traffic
* Try Parallel scans rather than the default sequential scan
* Avoid using scan operations if you can: design tables in a way that you can use the Query, Get or BatchGetItem APIs
* ***Throughput***
* Provision Throughput is measured in Capacity Units
* 1 x Write Capacity Unit = 1 x 1 KB Write per second
* 1 x Read Capacity Unit = 1 x 4 KB Strongly Consistent Read OR 
	*                     2 x 4 KB Eventually consistent Reads per seconds
* 


* 


<!--stackedit_data:
eyJoaXN0b3J5IjpbMzI4MzY3NTUsMTIyNDEyMzk4MCw5Nzg1Nz
QzMjksLTE4NDMwMDY2NywtMTE5MDc2MzU5NSw2MTc4MTk1OTMs
LTI1MTc5NTkxMSwtMjU2NTI3ODA5LDU3NDExODgxMSwxMDY5OT
c1NzUzLDc1Mzk3MTkyMywxMDkzOTU1ODUyXX0=
-->