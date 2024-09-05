# Design of a Distributed Messaging Queue: Part 2

Learn about the detailed design of a messaging queue and its managements at the back-end servers.

> We'll cover the following:
>
> - Back-end service
>   - Primary-secondary model
>   - A cluster of independent hosts

In the previous lesson, we discussed the responsibilities of front-end servers and metadata services. In this lesson, we’ll focus on the main part of the design where the queues and messages are stored: the back-end service.

## Back-end service

This is the core part of the architecture where major activities take place. When the front-end receives a message, it refers to the metadata service to determine the host where the message needs to be sent. The message is then forwarded to the host and is replicated on the relevant hosts to overcome a possible availability issue. The message replication in a cluster on different hosts can be performed using one of the following two models:

1. Primary-secondary model
2. A cluster of independent hosts

Before delving into the details of these models, let’s discuss the two types of cluster managers responsible for queue management: internal and external cluster managers. The differences between these two cluster managers are shown in the following table.

![internal versus external cluster managers](./images/5-1-internal-versus-external-cluster-managers.png)

#### Primary-secondary model

In the primary-secondary model, each node is considered a primary host for a collection of queues. The responsibility of a primary host is to receive requests for a particular queue and be fully responsible for data replication. The request is received by the front-end, which in turn communicates with the metadata service to determine the primary host for the request.

For example, suppose we have two queues with the identities 101 and 102 residing on four different hosts A, B, C, and D. In this example, instance B is the primary host of queue 101 and the secondary hosts where the queue 101 is replicated are A and C. As the front-end receives message requests, it identifies the primary server from the internal cluster manager through the metadata service. The message is retrieved from the primary instance, which is also responsible for deleting the original message upon usage and all of its replicas.

As shown in the following illustration, the internal cluster manager is a component that’s responsible for mapping between the primary host, secondary hosts, and queues. Moreover, it also helps in the primary host selection. Therefore, it needs to be reliable, scalable, and performant.

![primary-secondary model of a distributed queue](./images/5-2-primary-secondary-model-of-distributed-queue.png)

#### A cluster of independent hosts

In the approach involving a cluster of independent hosts, we have several clusters of multiple independent hosts that are distributed across data centers. As the front-end receives a message, it determines the corresponding cluster via the metadata service from the external cluster manager. The message is then forwarded to a random host in the cluster, which replicates the message in other hosts where the queue is stored.

---

**Point to Ponder**
How does a random host within a cluster replicate data—that is, messages—in the queues on other hosts within the same cluster?

A: Each host consists of mapping between the queues and the hosts within a cluster, making the replication easier.

Assume that we have a cluster, say Y, having hosts A, B, and C. This cluster has two queues with IDs 101 and 103 stored on different hosts, as shown in the following table. This table is stored on each host within the cluster Y. When a random host receives a message, say host C, for a queue having ID 103, host C replicates this message on the other hosts where the queue 103 is stored, i.e., Node A and Node B.

![exmaple](./images/5-4-example.png)

---

The same process is applied to receive message requests from the consumer. Similar to the first approach, the randomly selected host is responsible for message delivery and cleanup upon a successful processing of the message.

Furthermore, another component called an external cluster manager is introduced, which is accountable for maintaining the mapping between queues and clusters, as shown in the following figure. The external cluster manager is also responsible for queue management and cluster assignment to a particular queue.

The following figure illustrates the cluster of independent hosts. There are two clusters, A and B, which consist of several nodes. The external cluster manager has the mapping table between queues and their corresponding cluster. Whenever a front-end receives a request for a queue, it determines the corresponding cluster for the queue and forwards the request to the cluster where the queue resides. The nodes within that cluster are responsible for storing and sending messages accordingly.

![a cluster of independent hosts that consists of distributed queues](./images/5-3-a-cluster-of-independent-hosts-that-consists-of-distributed-queue.png)

---

**Point to Ponder**

**What kind of anomalies can arise while replication messages on other hosts?**
A: There are two ways to replicate messages in a queue residing on multiple hosts.

- Synchronous replication
- Asynchronous replication

In synchronous replication, the primary host is responsible for replicating the message in all the relevant queues on other hosts. After acknowledgment from secondary hosts, the primary host then notifies the client regarding the reception of messages. In this approach, messages remain consistent in all queues replicas; however, it costs extra delay in communication and causes partial to no availability while an election is in progress to promote a secondary as a primary.

In asynchronous replication, once the primary host receives the messages, it acknowledges the client, and in the next step, it starts replicating the message in other hosts. This approach comes with other problems such as replication lag and consistency issues.

Based on the needs of an application, we can pick one or the other.

---

We have completed the design of a distributed messaging queue and discussed the two models for organizing the back-end server. We also described the management process of queues and how messages are processed at the back-end. Furthermore, we discussed how back-end servers are managed through different cluster managers.

In the next lesson, we discuss how our system fulfills the functional and non-functional requirements that were described earlier in the chapter.
