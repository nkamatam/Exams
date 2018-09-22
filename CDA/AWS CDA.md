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
*  


<!--stackedit_data:
eyJoaXN0b3J5IjpbMjY0MTAyNjMyLDU3NDExODgxMSwxMDY5OT
c1NzUzLDc1Mzk3MTkyMywxMDkzOTU1ODUyXX0=
-->