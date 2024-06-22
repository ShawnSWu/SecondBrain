---
title: Getting to Know Kafka (2)
description: Exploring the core of fundamentals
date: 2023-08-25 00:00:00+0000
image: images/apache-kafka.jpg
categories:
    - kafka
---

## What are Kafka characteristic?

1. **{{< font color="#E6CC93" size="18px">}}Horizontal Scalability and High Availability{{< /font >}}**  
 Achieved through partitioning and replication mechanisms.
   
2. **{{< font color="#E6CC93" size="18px">}}Data Consistency{{< /font >}}**: Ensured by using a log mechanism for data persistence and replication.
   
3. **{{< font color="#E6CC93" size="18px">}}Separation of Data Storage and Consumption{{< /font >}}**: Producers generate data while consumers read and process data independently, enhancing system flexibility.

4. **{{< font color="#E6CC93" size="18px">}}Decoupling Producers and Consumers{{< /font >}}**: Producers send data to Kafka without worrying about the consumers. Consumers pull data from Kafka as needed, known as the pull model, which decouples the dependency between the two and increases system flexibility.



## Kafka is Pull Model

Kafka uses a pull model for data consumption, {{< font color="#ff8247" size="18px">}}which means that consumers actively request data from Kafka rather than Kafka pushing data to consumers.{{< /font >}}

> **Imagine if Kafka were to use a push model where machines send tens of thousands of data to Kafka and Kafka immediately forwards it to consumers,    
the consumers' computers would likely crash due to performance issues throughout the day.**


### How the Pull Model Works

1. Producers generate data and send it to Kafka topics. They do not need to know how or when this data will be consumed.
   
2. Consumers subscribe to Kafka topics and pull data from these topics as needed. They send requests to Kafka brokers, asking for new records from specific partitions.

### Benefits of the Pull Model

1. **{{< font color="#E6CC93" size="19px">}}Consumer-Controlled Flow{{< /font >}}**: Consumers control the rate at which they consume data. They can pull data at their own pace, which helps in managing and balancing load.
   
2. **{{< font color="#E6CC93" size="19px">}}Efficient Resource Utilization{{< /font >}}**: By pulling data, consumers can ensure they are ready to process the data, avoiding potential overload or resource contention.

3. **{{< font color="#E6CC93" size="19px">}}Flexibility in Processing{{< /font >}}**: Consumers can decide how much data to pull and process in each request, allowing them to adapt to varying workloads and processing capabilities.

4. **{{< font color="#E6CC93" size="19px">}}Improved Fault Tolerance{{< /font >}}**: In a pull model, if a consumer fails or needs to restart, it can simply resume pulling data from where it left off, reducing the risk of data loss or duplication.


Using Kafka's pull model, these updates are sent to Kafka topics by the machines (producers). Monitoring systems (consumers) then pull this data at their own pace, processing each update in real-time without being overwhelmed by the volume of data.

![Kafka Diagram 1](images/1.png)


## Key Roles in Kafka

### 1. **Broker**
Simply put, a broker is a Kafka node responsible for receiving, storing, and synchronizing data. Each broker not only serves consumers but also communicates with other brokers for data backup, forming a distributed cluster.

### 2. **Topic**
   In Kafka,  {{< font color="#E6CC93" size="19px">}}topic is like a category where producers send their messages.{{< /font >}} It acts as a central hub where producers put out data, and consumers sign up to get and use that data.

### 3. **Producer**
   {{< font color="#E6CC93" size="19px">}}Producers are responsible for continuously sending data to Kafka topics{{< /font >}}, with each piece of data considered a record. Producers can be various applications, such as monitoring systems, web server logs, or real-time data from factory machines.

### 4. **Consumer**
   {{< font color="#E6CC93" size="19px">}}Consumers can pull "message" at their own pace.{{< /font >}} They can operate independently or as part of a consumer group, dividing consumption tasks among the group members based on partitions.


Conceptual diagram:

![Kafka Diagram 2](images/2.png)

