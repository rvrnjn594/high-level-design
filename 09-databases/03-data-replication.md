# Data Replication

Understand the models through which data is replicated across several nodes.

> We'll cover the following:
>
> - Replication
> - Synchronous versus asynchronous replication
> - Data replication models
>   > - Single leader/primary-secondary replication
>   >   > - Primary-secondary replication methods
>   >   >   > - Statement-based replication
>   >   >   > - Write-ahead log (WAL) shipping
>   >   >   > - Logical (row-based) replication
>   > - Multi-leader replication
>   >   > - Conflict
>   >   > - Handle conflicts
>   >   >   > - Conflict avoidance
>   >   >   > - Last-write-wins
>   >   >   > - Custom logic
>   >   > - Multi-leader replication topologies
>   > - Peer-to-peer/leaderless replication
>   >   > - Quorums

> Data is an asset for an organization because it drives the whole business.  
> Data provides critical business insights into what’s important and what needs to be changed.  
>  Organizations also need to securely save and serve their clients’ data on demand.  
>  Timely access to the required data under varying conditions (increasing reads and writes, disks and node failures, network and power outages, and so on) is required to successfully run an online business.

We need the following characteristics from our data store:

- **Availability under faults** (failure of some disk, nodes, and network and power outages).
- **Scalability** (with increasing reads, writes, and other operations).
- **Performance** (low latency and high throughput for the clients).

It’s challenging, or even impossible, to achieve the above characteristics on a single node.

## Replication

Replication refers to **keeping multiple copies of the data at various nodes** (preferably geographically distributed) to achieve availability, scalability, and performance.

> In this lesson, we assume that a single node is enough to hold our entire data.  
>  We won’t use this assumption while discussing the partitioning of data in multiple nodes.

Often, **the concepts of replication and partitioning go together.**

> However, with many benefits, like availability, replication comes with its complexities.
>
> Replication is relatively simple if the replicated data doesn’t require frequent changes.  
>  The **main problem in replication arises when we have to maintain changes in the replicated data over time.**
>
> Additional complexities that could arise due to replication are as follows:
>
> - How do we keep multiple copies of data consistent with each other?
> - How do we deal with failed replica nodes?
> - Should we replicate synchronously or asynchronously?
>   - How do we deal with replication lag in case of asynchronous replication?
> - How do we handle concurrent writes?
> - What consistency model needs to be exposed to the end programmers?

We'll explore the answer to these questions in this lesson.  
 ![replication in action](./images/3-1-replication-in-action.png)

## Synchronous versus asynchronous replication

There are two ways to disseminate changes to the replica nodes:

- Synchronous replication
- Asynchronous replication

> In synchronous replication, **the primary node waits for acknowledgments from secondary nodes about updating the data.**  
>  After receiving acknowledgment from all secondary nodes, the primary node reports success to the client.
>
> Whereas in asynchronous replication, **the primary node doesn’t wait for the acknowledgment from the secondary nodes** and reports success to the client after updating itself.
>
> The **advantage of synchronous replication** is that all the secondary nodes are completely up to date with the primary node.  
> However, there’s a disadvantage to this approach.  
>  If one of the secondary nodes doesn’t acknowledge due to failure or fault in the network, the primary node would be unable to acknowledge the client until it receives the successful acknowledgment from the crashed node.
>
> This **causes high latency in the response from the primary node to the client.**
>
> On the other hand, **the advantage of asynchronous replication** is that the primary node can continue its work even if all the secondary nodes are down.  
>  However, if the primary node fails, the writes that weren’t copied to the secondary nodes will be lost.

The above paragraph explains a **trade-off between data consistency and availability** when components of the system can fail.

![synchronous versus asynchronous replication](./images/3-2-synchronous-versus-asynchronous-replication.png)

---

In the context of a real-time financial trading platform where low latency is critical and some degree of eventual consistency is acceptable, asynchronous updates are recommended. This approach allows the primary node to continue operations and report success after updating itself, reducing latency.  
 **Asynchronous replication is favored over synchronous replication because it mitigates potential latency issues, especially beneficial in environments requiring swift data updates.**

---

## Data replication models

Now, let's discuss various mechanisms of data replication.  
In this section, we'll discuss the following models along with their strengths and weakness:

- Single leader or primary-secondary replication
- Multi-leader replication
- Peer-to-peer or leaderless replication

### 1. Single leader/primary-secondary replication

In primary-secondary replication, data is replicated across multiple nodes.  
 One node is designated as the primary. It's responsible for processing any writes to data stored on the cluster.  
 It also sends all the writes to the secondary nodes and keeps them in sync.

Primary-secondary replication is **appropriate when our workload is read-heavy**.  
 To better scale with increasing readers, we can add more followers and distribute the read load across the available followers.  
 However, replicating data to many followers can make a primary bottleneck.  
 Additionaly, primary-secondary replication is inappropriate if our workload is write-heavy.

Another advantage of primary-secondary replication is that it's read resilient.  
 Secondary nodes can still handle read requests in case of primary node failure.  
 Therefore, it's helpful approach for read-intensive applications.

Replication via this approach comes with inconsistency if we use asynchronous replication.  
 Clients reading from different replicas may see inconsistent data in the case of failure of the primary node that couldn't propagate updated data to the secondary nodes.  
 So, if the primary node fails, any missed updates not passed on to the secondary nodes can be lost.

![primary-secondary data replication model where data is replicated from primary to secondary](./images/3-3-primary-secondary-data-replication-model-where-data-is-replicated-from-primary-to-secondary.png)

> WHAT HAPPENS WHEN THE PRIMARY NODE FAILS?
>
> In case of failure of the primary node, a secondary node can be appointed as a primary node, **which speeds up the process of recovering the initial primary node**.
>
> There are two approaches to select the new primary node:  
>  **manual and automatic**.
>
> In a **manual approach**, an operator decides which node should be the primary node and notifies all secondary node.
>
> In an **automatic approach**, when secondary nodes find out that the primary node has failed, they appoint the new primary node by conducting an election known as **leader election.**

#### Primary-secondary replication methods

There are many different replication methods in primary-secondary replication:

- Statement-based replication
- Write-ahead log (WAL) shipping
- Logical (row-based) replication  
   Let's discuss each of them in detail.

##### Statement-based replication

##### Write-ahead log (WAL) shipping

##### Logical (row-based) replication

### 2. Multi-leader replication

### 3. Peer-to-peer or leaderless replication
