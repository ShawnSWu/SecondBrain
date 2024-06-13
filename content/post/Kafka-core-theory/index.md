---
title: Kafka core theory
description: Kafka core theory
date: 2023-08-24 00:00:00+0000
categories:
    - Kafka
---

.  一個Broker裏面長怎樣?


> 一個Broker會管理著不同的Topic

每個Topic都會有以下兩個概念
1. Partition
2. Replica

> 想像一家麥當勞，漢堡就相當於Broker中的資料(Message)。

**Partition** 就好比麥當勞的服務窗口。如果只有一個窗口,當顧客太多時,排隊的人就會排得很長很長。所以麥當勞會開多個服務窗口(**Partition**), 這樣顧客就可以平均分散到不同窗口排隊,加快了服務效率。

生產者就好比麥當勞的廚房,它們生產出漢堡(**Message**),並均勻地分配到不同窗口。消費者就相當於顧客,他們只需要去自己排隊的那個窗口取餐,不同窗口的顧客是並行的,誰也不會影響別人。增加窗口數量,就能提高整體的服務能力。

另一方面,有些窗口可能會因故暫時關閉維修,那麼所有的顧客都只能去其他還開著的窗口取餐了。為了應付這種情況，麥當勞會為每個窗口(**Partition**)備份幾個一模一樣的窗口(**Replica**)。
當有一個主窗口(Leader Replica)關閉時,它旁邊的備份窗口(Follower Replica)就會立即接手工作，繼續為顧客服務,這樣顧客們就不會因為某個窗口關閉而受影響了。

> 點擊查看各自的詳細
### 1. [[Partition (分區)]]

- 在Kafka中，一個主題（topic）可以被分成多個 Partition (分區)。
- 分區是Message的基本單位，每個 Partition (分區) 都是有序的Message序列。
- 每個 Partition (分區) 在物理上都被保存在一個或多個伺服器上，以實現容錯性和擴展性。

正是因為這樣 Partition (分區) 機制，才允許Kafka集群可以水平擴展，提高了消息處理的吞吐量， 通過將消息分散到多個分區中，Kafka能夠在多個伺服器上平衡負載，實現分佈式消息處理。

### 2. [[Replica (副本)]]

- 在Kafka中，每個 Partition (分區) 都可以有一個或多個 Replica (副本)。
- Replica (副本) 是對 Partition (分區) 內容的複製，它們被保存在Kafka集群中的不同伺服器上。

正是因為 Replica (副本)的機制，提高了系統的可靠性和容錯性。當一個伺服器失效時，可以從其它伺服器上的 Replica (副本) 中恢復。

> [!important] 
> 正是透過 **Partition**和**Replica**的設計,Kafka實現了水平擴展、負載均衡和容錯機制。
**Partition**是水平礦展消息處理的基礎,而**Replica**則提供了備份和自動故障轉移能力。
> 
> 透過這種方式，一個Broker本身不只紀錄自己的資料，也需要幫別的Broker備份資料，這就是為什麼有其中幾個節點掛掉後，系統本身還可以正常運作，因為其他Broker可以把責任給扛起來


概念圖如下：
{{< figure src="images/1.png"  height="500" width="600">}}
