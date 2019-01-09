## Data Stores
Primarily focussing on New Solutions and Existing Solutions - also touch upon Migration solutions for Data Storage. We will be concerning with relaibility, continuity regarding storage - both traditional storage and databases. Most important usecase is to migrating on-prem data store (NAS or SAN)to AWS data store. We will also cover some of the migration strategies here. Becasue each of the data layers has different cost, we will aslo get into the cost component also. 

### Concepts:
#### Difference between the data stores:
1. Persistent Data Store - Data is durable and sticks around after reboots, restarts or power cycles (Glacier, RDS, S3)
2. Transient Data Store - Data is temp stored and passed along to another process or persistent store (SQS, SNS)
3. Ephemeral Data Store - Data is lost when stopped ( EC2 insrance store, Memchached)

#### Difference between IOPS vs Throughput
**IOPS** : Input Outpus per Second : Measure of how fast we can read and write to a device
**Throughput** : Measure of how much data can be moved at a time

**Consistency**: Ground rule that a datastore plays by so that we can be sure that we can be sure that the data handed to  AWS is going to behave in certain ways.

#### Consistency Models - ACID vs BASE
ACID - Atomic (transaction are all or nothing), Consistency (Transactions must be valid - cannot be corrupted), Isolated (Tranactions cannot mess with one another) and Durability(Completed transaction must stick around - ability to commit to the database)
BASE - Basic availability (values availability even if it is stale), Soft-state (might not be instantly consistent accross stores), Eventual consistency - (Will achive consistency at some point)



BASE - 
 





 

