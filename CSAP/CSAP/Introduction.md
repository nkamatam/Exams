---


---

<h2 id="data-stores">Data Stores</h2>
<p>Primarily focussing on New Solutions and Existing Solutions - also touch upon Migration solutions for Data Storage. We will be concerning with relaibility, continuity regarding storage - both traditional storage and databases. Most important usecase is to migrating on-prem data store (NAS or SAN)to AWS data store. We will also cover some of the migration strategies here. Becasue each of the data layers has different cost, we will aslo get into the cost component also.</p>
<h3 id="concepts">Concepts:</h3>
<h4 id="difference-between-the-data-stores">Difference between the data stores:</h4>
<ol>
<li>Persistent Data Store - Data is durable and sticks around after reboots, restarts or power cycles (Glacier, RDS, S3)</li>
<li>Transient Data Store - Data is temp stored and passed along to another process or persistent store (SQS, SNS)</li>
<li>Ephemeral Data Store - Data is lost when stopped ( EC2 insrance store, Memchached)</li>
</ol>
<h4 id="difference-between-iops-vs-throughput">Difference between IOPS vs Throughput</h4>
<p><strong>IOPS</strong> : Input Outpus per Second : Measure of how fast we can read and write to a device<br>
<strong>Throughput</strong> : Measure of how much data can be moved at a time</p>
<p><strong>Consistency</strong>: Ground rule that a datastore plays by so that we can be sure that we can be sure that the data handed to  AWS is going to behave in certain ways.</p>
<h4 id="consistency-models---acid-vs-base">Consistency Models - ACID vs BASE</h4>
<p>ACID - Atomic (transaction are all or nothing), Consistency (Transactions must be valid - cannot be corrupted), Isolated (Tranactions cannot mess with one another) and Durability(Completed transaction must stick around - ability to commit to the database)<br>
BASE - Basic availability (values availability even if it is stale), Soft-state (might not be instantly consistent accross stores), Eventual consistency - (Will achive consistency at some point)</p>
<p>BASE -</p>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzMTA1NDAwNV19
-->