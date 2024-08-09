# Final Design of Quora

Learn about the limitations of Quora's design and improve the design.

> We'll cover the following:
>
> - Limitations of the proposed design
> - Detailed design of Quora
>   - Service hosts
>   - Vertical sharding of MySQL
>   - MyRocks
>   - Kafka
>   - Technology usage

## Limitations of the proposed design

The proposed design serves all the functional requirements.  
 However, it has a number of serious drawbacks that emerge as we scale. This means that we are unable to fulfill the non-functional requirements.

Let's explore the main shortcomings below:

- **Limitations of web and application servers:**
  To entertain the user’s request, payloads are transferred between web and application servers, which increases latency because of network I/O between these two types of servers.  
  Even if we achieve parallel computation by separating the web from application servers (that is, the manager and worker processes), the added latency due to an additional network link erodes a user’s experience.  
  Apart from data transfer, control communication between the router library with manager and worker processes also imposes additional performance penalties.
- **In-memory queue failure:**
  The internal architecture of application servers log tasks and forward them to the in-memory queues, which serve them to the workers. These in-memory queues of different priorities can be subject to failures.  
   For instance, if a queue gets lost, all the tasks in that queue are lost as well, and manual engineering is required to recover those tasks. This greatly reduces the performance of the system. On the other hand, replicating these queues requires increasing RAM size. Also, with the number of features (functional requirements) that our system offers, many tasks can get assembled, which results in insufficient memory. At the same time, it is not desirable to choke application servers with not-so-urgent tasks. For example, application servers should not be burdened with tasks like storing view counts for answers, adding statistics to the database for later analysis, and so on.
- **Increasing QPS on MySQL:**
- **Latency of HBase:**

## Detailed design of Quora

#### Service hosts

#### Vertical sharding of MySQL

#### MyRocks

#### Kafka

#### Technology usage
