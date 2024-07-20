# Data Partitioning

Learn about data partitioning models along with their pros and cons.

> We'll cover the following:
>
> - Why do we partition data?
> - Sharding
>   - Vertical sharding
>   - Horizontal sharding
>     - Key-range based sharding
>     - Hash-based sharding
>     - Consistent hashing
>   - Rebalance the partitions
>     - Avoid hash mod n
>     - Fixed number of partitions
>     - Dynamic partitioning
>     - Partition proportionally to nodes
>   - Partitioning and secondary indexes
>     - Partition secondary indexes by document
>     - Partition secondary indexes by the term
> - Request routing
>   - ZooKeeper
> - Conclusion

## Why do we partition data?

> Data is an asset for any organization. Increasing data and concurrent read/write traffic to the data puts scalability pressure on traditional databases.  
>  As a result, the latency and throughput are affected.  
>  Traditional databases are attractive due to their properties such as range queries, secondary indices, and transactions with the ACID properties.
>
> At some point, a single node-based database isn’t enough to tackle the load.  
>  We might need to distribute the data over many nodes but still export all the nice properties of relational databases.  
>  In practice, it has proved challenging to provide single-node database-like properties over a distributed database.
>
> One solution is to move data to a NoSQL-like system.  
>  However, the historical codebase and its close cohesion with traditional databases make it an expensive problem to tackle.
>
> Organizations might scale traditional databases by using a third-party solution.  
>  But often, integrating a third-party solution has its complexities.  
>  More importantly, there are abundant opportunities to optimize for the specific problem at hand and get much better performance than a general-purpose solution.

Data partitioning (or sharding) enables us to **use multiple nodes where each node manages some part of the whole data.**  
 To handle increasing query rates and data amounts, we strive for balanced partitions and balanced read/write load.

We'll discuss different ways to partition data, related challenges, and their solutions in this lesson.  
 ![A database with two partitions to distribute the data and associated read/write load](./images/4-1-a-database-with-two-partitions-to-distribute-the-data-and-associated-read-write-load.png)

## Sharding

> To divide load among multiple nodes, we need to **partition the data by a phenomenon known as partitioning or sharding**.  
>  In this approach, we **split a large dataset into smaller chunks of data** stored at different nodes on our network.

The partitioning must be balanced so that each partition receives about the same amount of data.  
 **If partitioning is unbalanced**, _the majority of queries will fall into a few partitions._

Partitions that are heavily loaded will create a system bottleneck.  
The efficacy of partitioning will be harmed because a significant portion of data retrieval queries will be sent to the nodes that carry the **highly congested partitions**.  
 **Such partitions are known as hotspots.**

Generally, we use the following two ways to shard the data:

- Vertical sharding
- Horizontal sharding

### Vertical sharding

We can put different tables in various database instances, which might be running on a different physical server.  
 We might break a table into multiple tables so that **some columns are in one table while the rest are in the other.**

We should be careful if there are joins between multiple tables.  
We may like to keep such tables together on one shard.

Often, vertical sharding is used to increase the speed of data retreival from a **table consisting of columns with very wide text or a binary large object** **_(blob)._**  
 In this case, the column with large text or a blob is split into a different table.

> As shown in the figure a couple paragraphs below, the Employee table is divided into two tables: a reduced Employee table and an EmployeePicture table.
>
> The EmployeePicture table has just two columns, EmployeeID and Picture, separated from the original table.
>
> Moreover, the primary key EmployeeID of the Employee table is added in both partitioned tables.
>
> This makes the data read and write easier, and the reconstruction of the table is performed efficiently.

Vertical sharding has its intracacies and is **more amenable to manual partitioning**, where stakeholders carefully decide how to partition data.  
 In comparison, horizontal sharding is suitable to automate even under dynamic conditions.

![Vertical partitioning](./images/4-2-vertical-partitioning.png)

> **NOTE:** **Creating shards by moving specific tables of a database around** is also a form of vertical sharding.  
>  Usually, **those tables are put in the same shard because they often appear together in queries**, for example, _for joins._  
>  We will see an example of such a use-case ahead in the course.

### Horizontal sharding

At times, some tables in the databases become too big and affect read/write latency.  
 Horizontal sharding or partitioning is used to **divide a table into multiple tables by splitting data row-wise**, as shown in the figure in the next section.

**Each partition of the original table distributed over databases servers** is called a shard.  
Usually, there are two strategies available:

- Key-range based sharding
- Hash based sharding

##### Key-range based sharding

In the key-range based sharding, each partition is assigned a continuous range of keys.

> In the following figure, horizontal partitioning on the Invoice table is performed using the key-range based sharding with Customer_Id as the partition key.  
>  The two different colored tables represent the partitions.
>
> ![horizontal partitioning](./images/4-4-horizontal-partitioning.png)

Sometimes, a database consists of multiple tables bound by foreign key relationships.  
 In such a case, the horizontal partition is performed using the same partition key on all tables in a relation.  
 Tables (or subtables) that belong to the same partition key are distributed to one database shard.

The following figure shows that several tables with the same partition key are placed in a single database shard:

![horizontal partitioning on a set of tables](./images/4-5-horizontal-partitioning-on-a-set-of-tables.png)

> The basic design techniques used in multi-table sharding are as follows:
>
> - There’s a partition key in the Customer mapping table. This table resides on each shard and stores the partition keys used in the shard.  
>    Applications create a mapping logic between the partition keys and database shards by reading this table from all shards to make the mapping efficient.  
>    Sometimes, applications use advanced algorithms to determine the location of a partition key belonging to a specific shard.
>
> - The partition key column, Customer_Id, is replicated in all other tables as a data isolation point.  
>    It has a trade-off between an impact on increased storage and locating the desired shards efficiently. Apart from this, it’s helpful for data and workload distribution to different database shards. The data routing logic uses the partition key at the application tier to map queries specified for a database shard.
>
> - Primary keys are unique across all database shards to avoid key collision during data migration among shards and the merging of data in the online analytical processing (OLAP) environment.
>
> - The column Creation_date serves as the data consistency point, with an assumption that the clocks of all nodes are synchronized. This column is used as a criterion for merging data from all database shards into the global view when essential.

###### Advantages

- Using key-range-based sharding method, the **range-query-based scheme is easy to implement**. We precisely know where (which node, which shard) to look for a specific range of keys.
- Range queries **can be performed using the partitioning keys**, and those can be kept in partitions in sorted order. How exactly such a sorting happens over time as new data comes in is implementation specific.

###### Disadvantages

- Range queries **can’t be performed using keys other than the partitioning key**.
- If keys aren’t selected properly, some nodes may have to store more data due to an uneven distribution of the traffic.

##### Hash-based sharding

Hash-based sharding **uses a hash function on an attribute.**  
 This hash function produces a hash value that is used to perform partitioning.

The main concept is to **use a hash function on the key to get a hash value and then mod by the number of partitions.**  
 Once we’ve found an appropriate hash function for keys, we may give each partition a range of hashes (rather than a range of keys).  
 Any key whose hash occurs inside that range will be kept in that partition.

> In the illustration below, we use a hash function of Value mod = n.  
>  The n is the number of nodes, which is four. We allocate keys to nodes by checking the mod for each key. Keys with a mod value of 2 are allocated to node 2.  
>  Keys with a mod value of 1 are allocated to node 1.
>
> Keys with a mod value of 3 are allocated to node 3.  
>  Because there's no key with a mod value of 0, node 0 is left vacant.
>
> ![hashed-based sharding](./images/4-6-hash-based-sharding.png)

###### Advantages

- Keys are uniformly distributed across the nodes.

###### Disadvantages

- We can't perform range queries with this technique.  
   Keys will be spread over all partitions.

---

> ##### Why do you need databases? Why can't you just use files?
>
> Databases indeed **offer optimized resource usage**, **support complex joins**, **provide faster throughput**, and **ensure low latency**, among other benefits.  
>  Additionally, databases are crucial for managing **large volumes of data efficiently,** **ensuring data consistency and security**, **facilitating easy updates**, and **supporting scalability and data recovery**.

> **NOTE:** How many shards per databases?
>
> Emperically, we can determine how much each node can serve with acceptable performance.  
>  It can help us find out maximum amount of data that we would like to keep on any one node.
>
>       For example, if we find out that we can put a maximum of 50 GB of data on one node, we have the following:
>
> Database size = 10 TB  
> Size of a single shard = 50 GB
>
> Number of shards the database should be distributed in = 10 TB / 50 GB = 200 shards.

---

##### Consistent hashing

Consistent hashing **assigns each server or item in a distributed hash table a place on an abstract circle,** _called a ring,_ irrespective of the number of servers in the table.  
 This permits servers and objects to scale without compromising the system's overall performance.

###### Advantages of consistent hashing

- It's easy to scale horizontally.
- It increases the throughput and improves the latency of the application.s

###### Disadvantages of consistent hashing

- Randomly assigning nodes in the ring may cause non-uniform distributions.

### Rebalance the partitions

Query load can be imbalanced across the nodes due to many reasons, including the following:

- The distribution of the data isn't equal.
- There's too much load on a single partition.
- There's an increase in the query traffic, and we need to add more nodes to keep up.

We can apply the following strategies to rebalance partitions.

##### Avoid hash mod n

##### Fixed number of partitions

##### Dynamic partitioning

##### Partition proportionally to nodes

### Partitioning and secondary indexes

##### Partition secondary indexes by document

##### Partition secondary indexes by the term

## Request routing

We'v

### Zookeeper

## Conclusion

For all current distributed systems, partitioning has become the standard protocol.  
Because systems contain increasing amounts of data, partitioning the data makes sense since it speeds up both writes and reads.  
 It increases the system's availability, scalability, and performance.
