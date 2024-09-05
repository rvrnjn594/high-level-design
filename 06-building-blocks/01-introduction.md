# Introduction to Building Blocks for Modern System Design

Learn how a system design is like using Lego pieces to make bigger, fascinating artifacts.

> We'll cover the following:
>
> - The bottom-up approach for modern system design
> - Conventions

## The bottom-up approach for modern system design

System design problems ususally have some similarities, though specific details are often unique. These similarities across design problems are extracted as **"basic building blocks".** (example: load-balancing component)

> Purpose of seperating the building blocks is to thoroughly discuss their design just once, and later use them anywhere without having to go over them in detail again.
>
> Think about building blocks as **bricks to construct more effective, capable systems.**
>
> Many of the building blocks are also available for actual use in public clouds, such as AWS, Azure, and Google Cloud Platform (GCP).

We'll discuss the following building blocks in detail:

1. **Domain Name System:**  
   This building block focuses on how to design hierarchical and distributed naming systems for computers connected to the Internet via different Internet protocols.
2. **Load Balancers:**  
   Understand the design of a load balancer, which is used to fairly distribute incoming client's requests among a pool of available servers.  
   (It also reduces load and can bypass failed servers.)
3. **Databases:**  
   This building block enables us to store, retrieve, modify, and delete data in connection with different data-processing procedures.  
   (discuss database types, replication, partitioning, and analysis of distributed databases.)
4. **Key-Value Store:**  
   It is a non-relational database that stores data in the form of a key-value pair.  
   (explain design of a key-value store along with important concepts such as achieving scalability, durability, and configurability.)
5. **Content Delivery Network:**  
   Desing a content delivery network (CDN) that's used to keep viral content such as videos, images, audio, and webpages.  
   (efficiently delivers content to end users while reducing latency and burden on the data centers.)
6. **Sequencer:**  
   Focus on design of a unique IDs generator with a major focus on maintaining causlity.  
   (explains three different unique IDs.)
7. **Service Monitoring:**  
   Monitoring systems are critical in distributed systems because they help analyze the system and alert the stakeholders if a problem occurs.  
   Monitoring is often useful to get early warning systems so that system administrators can act ahead of an impending problem becoming a huge issue.  
   (Build two monitoring systems, one for the server-side and the other for client-side errors.)
8. **Distributed Caching:**  
   We'll design a distributed caching system where multiple cache servers coordinate to store frequently accessed data.
9. **Distributed Messaging Queue:**  
   Focus on the design of a queue consisting of multiple servers, which is used between interacting entitites called producers and consumers.  
   (It helps decouple producers and consumers, results in independent scalabilty, and enhances reliability.)
10. **Publish-Subscribe System:**  
    Focus on the design of an asynchronous service-to-service communication method called a pub-sub system.  
    (popular in serverless, microservices architectures and data processing systems.)
11. **Rate Limiter:**  
    Design a system that throttles incoming requests for a service based on the predefined limit.  
    (generally used as a defensive layer for services to avoid their excessive usage-whether intended or unintended.)
12. **Blob Store:**  
    The building block focuses on a storage solution for unstructured data - for example, multimedia files and binary executables.
13. **Distributed Search:**  
    A search system takes a query from a user and returns relevant content in a few seconds or less.  
    (focuses on the three integral components: crawl, index, and search.)
14. **Distributed Logging:**  
    Logging is an I/O intensive operation that is time-consuming and slow.  
    (design a system that allows services in a distributed system to log their events efficiently.)  
    The system will be made scalable and reliable.
15. **Distributed Task Scheduling:**  
    Design a distributed task scheduler system that mediates between tasks and resources.  
    (intelligently allocates resources to tasks to meet task-level and system-level goals.)
    > It's often used to offload background processing to be completed asynchronously.
16. **Sharded Counters:**  
    This building block demonstrates an efficient distributed counting system to deal with millions of concurrent read/write requests, such as likes on a celebrity's tweet.

> We have topologically ordered the building blocks so the building blocks that depend on others come later.

## Conventions

For elaboration, we'll use a _"Requirements"_ section whenever we design a building block (and a design problem).  
The _"Requirements"_ section will highlight the deliverables we expect from the developed design.

_"Requirements"_ will have two sub-categories:

1. **Functional Requirements:**  
   These represent the features a user of the designed system will be able to use.  
   (example: the sytem allow a user to search for content using the search bar.)
2. **Non-functional Requirements (NFRs):**  
   The non-functional requirements are criteria based on which the user of a system will consider the system usuable.  
   (NFR may include requirements like high availability, low latency, scalability, and so on...)

> Let's start with our building blocks.....
