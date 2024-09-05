# Design of a Monitoring System

Learn about the initial design of a generic monitoring sysytem.

> We'll cover the following:
>
> - Requirements
> - Building block we will use
>   - High level design

## Requirements

Let’s sum up what we want our monitoring system to do for us:

- Monitor critical local processes on a server for crashes.
- Monitor any anomalies in the use of CPU/memory/disk/network bandwidth by a process on a server.
- Monitor overall server health, such as CPU, memory, disk, network bandwidth, average load, and so on.
- Monitor hardware component faults on a server, such as memory failures, failing or slowing disk, and so on.
- Monitor the server’s ability to reach out-of-server critical services, such as network file systems and so on.
- Monitor all network switches, load balancers, and any other specialized hardware inside a data center.
- Monitor power consumption at the server, rack, and data center levels.
- Monitor any power events on the servers, racks, and data center.
- Monitor routing information and DNS for external clients.
- Monitor network links and paths’ latency inside and across the data centers.
- Monitor network status at the peering points.
- Monitor overall service health that might span multiple data centers—for example, a CDN and its performance.

> We want automated monitoring that identifies an anomaly in the system and informs the alert manager or shows the progress on a dashboard. Cloud service providers provide a health status of their services:
>
> - AWS: https://health.aws.amazon.com/health/status
> - Azure: https://status.azure.com/en-us/status
> - Google: https://status.cloud.google.com/

---

In a distributed system, why is a dedicated monitoring solution necessary instead of simply relying on individual server logs?

Certainly! In a distributed system, a dedicated monitoring solution is necessary for several key reasons:

1. Centralized visibility across the entire system: It provides a unified view of the health and performance of the entire distributed system rather than isolated snapshots from individual servers.
2. Detecting correlations and patterns that individual logs would miss: By analyzing data across different parts of the system, it can identify issues and patterns that are not visible when looking at single components in isolation.
3. Proactive alerts and faster troubleshooting: A dedicated monitoring system can be configured to send alerts on detecting anomalies or potential issues, enabling quicker response times and more effective troubleshooting.

These aspects are crucial for maintaining the reliability, performance, and availability of distributed systems.

---

## Building block we will use

The design of distributed monitoring will consist of the following building block:
**Blob Storage:** We'll use blob storage to store our information about metrics.

#### High level design

The high-level components of our monitoring service are the following:

- **Storage:**  
   A time-series database stores metrics data, such as the current CPU use or the number of exceptions in an application.
- **Data collector service:**  
   This fetches the relevant data from each service and saves it in the storage.
- **Query service:**  
   This is an API that can query on the time-series database and return the relevant information.

![high-level design of a monitoring system](./images/1-1-high-level-design-of-a-monitoring-system.png)

> Let's dive deep into the components mentioned above in the next lesson.
