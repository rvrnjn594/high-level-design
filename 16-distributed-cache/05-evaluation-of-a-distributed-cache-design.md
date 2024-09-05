# Evaluation of a Distributed Cache's Design

Let's evaluate our design in the context of our requirements.

> We'll cover the following
>
> - High performance
> - Scalability
> - High availability
> - Consistency
> - Affordability
> - Summary

Let’s evaluate our proposed design according to the design requirements.

## High performance

Here are some design choices we made that will contribute to overall good performance:

- We used consistent hashing. Finding a key under this algorithm requires a time complexity of O(log(N)), where N represents the number of cache shards.
- Inside a cache server, keys are located using hash tables that require constant time on average.
- The LRU eviction approach uses a constant time to access and update cache entries in a doubly linked list.
- The communication between cache clients and servers is done through TCP and UDP protocols, which is also very fast.
- Since we added more replicas, these can reduce the performance penalties that we have to face if there’s a high request load on a single machine.
- An important feature of the design is adding, retrieving, and serving data from the RAM. Therefore, the latency to perform these operations is quite low.

> Note: A critical parameter for high performance is the selection of the eviction algorithm because the number of cache hits and misses depends on it. The higher the cache hit rate, the better the performance.

![how important the eviction algorithm is](./images/5-1-how-important-the-eviction-algorithm-is.png)

## Scalability

We can create shards based on requirements and changing server loads. While we add new cache servers to the cluster, we also have to do a limited number of rehash computations, thanks to consistent hashing.

Adding replicas reduces the load on hot shards. Another way to handle the hotkeys problem is to do further sharding within the range of those keys. Although the scenario where a single key will become hot is rare, it’s possible for the cache client to devise solutions to avoid the single hotkey contention issue. For example, cache clients can intelligently avoid such a situation that a single key becomes a bottleneck, or we can use dynamic replication for specific keys, and so on. Nonetheless, the solutions are complex and beyond the scope of this lesson.

![guarantees provided by a distributed cache](./images/5-2-guarantees-provided-by-a-distributed-cache.png)

## High availability

We have improved the availability through redundant cache servers. Redundancy adds a layer of reliability and fault tolerance to our design. We also used the leader-follower algorithm to conveniently manage a cluster shard. However, we haven’t achieved high availability because we have two shard replicas, and at the moment, we assume that the replicas are within a data center.

It’s possible to achieve higher availability by splitting the leader and follower servers among different data centers. But such high availability comes at a price of consistency. We assumed synchronous writes within the same data center. But synchronous writing for strong consistency in different data centers has a serious performance implication that isn’t welcomed in caching systems. We usually use asynchronous replication across data centers.

For replication within the data center, we can get strong consistency with good performance. We can compromise strong consistency across data center replication to achieve better availability (see CAP and PACELC theorems).

## Consistency

It’s possible to write data to cache servers in a synchronous or asynchronous mode. In the case of caching, the asynchronous mode is favored for improved performance. Consequently, our caching system suffers from inconsistencies. Alternatively, strong consistency comes from synchronous writing, but this increases the overall latency, and the performance takes a hit.

Inconsistency can also arise from faulty configuration files and services. Imagine a scenario where a cache server is down during a write operation, and a read operation is performed on it just after its recovery. We can avoid such scenarios for any joining or rejoining server by not allowing it to serve requests until it’s reasonably sure that it’s up to date.

## Affordability

Our proposed design has a low cost because it’s feasible and practical to create such a system using commodity hardware.

---

**What happens if the leader node fails in the leader-follower protocol?**

Two possible solutions can be used in that case:

- Use the leader election algorithm to elect a new leader among the set of available followers.
- Use a separate distributed configuration management service to monitor and select leaders.

**Is it a good idea to introduce synchronous replication to cache servers?**

Synchronous replication on the cache clusters will introduce a performance penalty because it takes time to replicate the same data to all the cache servers. Therefore, there’s a trade-off between performance and consistency in the usage of asynchronous versus synchronous replication, respectively.

The answer depends on the use case. In case of cache servers, synchronous replication is a good choice if the replicated servers are within close proximity of the primary server.

**Assume that a large number of concurrent requests are forwarded to a single shard server. If the server uses the LRU algorithm, some of the requests will result in a cache miss and write a new entry to the cache, while some will result in a cache hit and end up reading from the server. These concurrent requests may compete with each other. Therefore, some locking mechanisms _(A mechanism by which access to shared data is limited to a single thread only. Thus achieving synchronization.)_ may be required. However, locking an entire data structure for all the reading and writing operations will heavily reduce the performance. How do you think such a problem will be dealt with?**

Generally, for concurrent access to shared data, some locking mechanisms like Semaphore, Monitors, Mutex locks, and others are good choices. But as the number of users (readers in case of cache hit or writers in case of a cache miss) grows, locking the entire data structure isn’t a suitable solution. In that case, we can consider the following options:

- Limited locking: In this strategy, only specific sections of the entire data structure will be locked. While some threads or processes can read from the data structure simultaneously, some threads may temporarily block access to specific sections of the data structure.
- Offline eviction: Offline eviction may be a possibility where instead of making actual changes to the data structure, only the required changes will be recorded while performing different operations until it’s necessary to commit the changes. This solution is desirable and easy if the cache hit rate is high because a change to the data structure will likely be required when a cache miss occurs.
- Lock-free implementation: Multiple solutions have been proposed which suggest that simultaneous reading and writing over doubly linked list is feasible to support large number of concurrent reading and writing.

---

## Summary

We studied the basics of the cache and designed a distributed cache that has a good level of availability, high performance, high scalability, and low cost. Our mechanism maintains high availability using replicas, though if all replicas are in one data center, such a scheme won’t tackle full data center failures. Now that we’ve learned about the basics of design, let’s explore popular open-source frameworks like Memcached and Redis.
