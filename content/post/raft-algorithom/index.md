---
title: Raft Algorithom
description: A kind of consensus Algorithom
slug: raft-algorithom
date: 2022-03-06 00:00:00+0000
image: cover.jpg
categories:
    - Example Category
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

不同於Paxos，Raft使用Leader(領導者)、Follower(追隨者)等更直觀的術語
並且簡化了複雜的流程，主要還是有三個流程

1. **Leader Election 領導者選舉**
2. **Log Replication 日誌複製**
3. **Log Re日誌一致性**

在Raft中，有以下三個角色代表不同節點
1. Follower
2. Candidate
3. Leader

{{< figure src="elephant.jpg" title="An elephant at sunset" >}}

![figure. 1](Raft_01.png)

## 1. Leader Election

1. **初始化狀態**： - 系統中的所有節點開始時都處於Follower狀態。 
      ![figure. 2](Raft_02.png)
	   
	2. **超時觸發選舉**： - 每個Follower節點在一定時間內沒有收到來自Leader的心跳訊號(Heartbeat)，它會轉變為Candidate並發起選舉，
	   
	   以下舉例為節點初始化時的狀態(沒任何Leader 傳 heaerbeat)，每個節點的進度條則代表沒收到心跳訊號的時間
 {{< figure src="3.gif" title="An elephant at sunset" >}}