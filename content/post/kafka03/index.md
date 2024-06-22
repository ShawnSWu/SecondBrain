---
title: Getting to Know Kafka (3)
description: Unpacking the Kafka broker
date: 2023-08-24 00:00:00+0000
image: images/apache-kafka.jpg
categories:
    - Kafka
---

# **{{< font color="#E6CC93" size="25px">}}A single broker in Kafka can handle multiple topics concurrently.{{< /font >}}**

**Each Topic have the following two concepts**

{{< font color="#82f571" size="20px">}}1. Partition{{< /font >}}    
{{< font color="#82f571" size="20px">}}2. Replica{{< /font >}}

> {{< font color="#E6CC93" size="20px">}}Imagine a McDonald's restaurant, each hamburger represents a piece of data (message) within a Kafka broker.{{< /font >}} 

{{< font color="#82f571" size="20px">}}Partition{{< /font >}} in Kafka are similar to {{< font color="#ff2e74" size="17px">}}service counters{{< /font >}} at McDonald's. If there is only one counter and too many customers, the queue can become very long. 
To improve service efficiency, {{< font color="#ff2e74" size="17px">}}McDonald's opens multiple service counters (partitions){{< /font >}} , allowing customers to spread out across different counters and speed up service.

{{< font color="#E6CC93" size="18px">}}Producers{{< /font >}} in Kafka are  {{< font color="#ff2e74" size="17px">}}like the kitchens in McDonald's{{< /font >}}.
they produce hamburgers (messages) and distribute them evenly across different counters. 


{{< font color="#E6CC93" size="18px">}}Consumers{{< /font >}} are {{< font color="#ff2e74" size="17px">}}like McDonald'scustomers{{< /font >}} who only need to go to their designated counter to get their bergers. 

Customers at different counters can be served simultaneously without affecting each other. Increasing the number of counters improves the overall service capacity.


On the other hand, some counter at McDonald's may temporarily close for maintenance, in such cases, all customers would need to go to the other okay counters. 

To handle this situation, McDonald's will back up several『service counter』(Replica) for each『service counter』(Partition).

{{< font color="#82f571" size="16px">}}These back up『service counter』 called replicas in Kafka terms.{{< /font >}}

When the main counter (leader replica) closes, one of its backup counters (follower replica) immediately takes over and continues serving customers. This ensures that customers are not affected by the closure of a particular counter.

### 1. Partition

- {{< font color="#E6CC93" size="17px">}}A topic can be divided into multiple partitions.{{< /font >}}
- {{< font color="#E6CC93" size="17px">}}Partitions as the basic unit of messages, each partition containing an ordered sequence of messages.{{< /font >}}
- {{< font color="#E6CC93" size="17px">}}Physically, each partition is stored on one or more servers to achieve fault tolerance and scalability.{{< /font >}}

It is precisely because of this mechanism (Partition), it allows Kafka clusters to horizontally scale, enhancing message throughput. By distributing messages across multiple partitions.

### 2. Replica

- {{< font color="#E6CC93" size="17px">}}In Kafka, each partition can have one or more replicas.{{< /font >}}
- {{< font color="#E6CC93" size="17px">}}A replica is a copy of the content within a partition, and replicas are stored on different servers within the Kafka cluster.{{< /font >}}


It is precisely because of the replica mechanism that system reliability and fault tolerance are enhanced.

{{< font color="#ff2e74" size="22px">}}When a server fails, data can be recovered from replicas stored on other servers.{{< /font >}}


Through the design of Partitions and Replicas, Kafka achieves horizontal scalability, load balancing, and fault tolerance. 

* Partitions is the foundation for horizontally scaling message processing 
* Replicas provide backup and automatic failover capabilities.

> In this way, each Broker in Kafka not only records its own data but also helps replicate data for other Brokers. 
This redundancy ensures that even if several nodes fail, the system can continue operating normally because other Brokers can take on the responsibility.

{{< figure src="images/1.png"  height="500" width="600">}}

### My thoughts
Conceptually not quite hard, but I feel the real core is the consensus algorithm between each node.