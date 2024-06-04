---
title: Raft Algorithom
description: A kind of consensus Algorithom
slug: hello-world
date: 2022-03-06 00:00:00+0000
image: cover.jpg
categories:
    - Example Category
tags:
    - Example Tag
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

不同於Paxos，Raft使用Leader(領導者)、Follower(追隨者)等更直觀的術語
並且簡化了複雜的流程，主要有三個流程：

1. **Leader Election 領導者選舉**
2. **Log Replication 日誌複製**
3. **Log Consistency 日誌一致性**

在Raft中，有以下三個角色代表不同節點：
1. Follower
2. Candidate
3. Leader

![Nodes overview](raft-nodes-overview.png)

## 1. Leader Election

1. **初始化狀態**
   - 系統中的所有節點開始時都處於Follower狀態。 
   ![1](1.png)	   

2. **超時觸發選舉**
   - 每個Follower節點在一定時間內沒有收到來自Leader的心跳訊號(Heartbeat)，它會轉變為Candidate並發起選舉。
   - 以下舉例為節點初始化時的狀態(沒任何Leader 傳 heaerbeat)，每個節點的進度條則代表沒收到心跳訊號的時間
   ![Leader election](leader_election.gif)

3. **發送投票請求**
   - Candidate節點向其他所有節點發送RequestVote請求，並附帶其當前的日誌索引和任期號。 
   ![2](2.gif)

4. **接受投票**
   - 其他節點（Followers）收到RequestVote請求後，會根據Candidate的日誌索引和任期號決定是否投票。
   - 如果該Candidate的日誌比自己更新，且尚未投票給其他Candidate，則會投票給該Candidate(這裡算是算法的核心之一)。
   - 像上面gif顯示的結尾部分，Candidate變成Leader。

5. **當選為Leader**
   - Candidate節點如果獲得多數節點的投票（超過半數），則成為Leader。
   - 當選後，它會立即向其他節點發送心跳訊號，通知其成為新的Leader，以下為正常Leader持續傳送心跳訊號的樣子。
   ![3](3.gif)

6. **處理失敗情況**
   - 如果Candidate在一定時間內沒有獲得足夠的投票，它會重新進入Follower狀態，並等待下一次選舉超時再次發起選舉。
   - 以下是當Leader掛掉 不再傳送心跳訊號時，各節點會再重新選出新Leader的選舉機制（意義上就是回到 **1.初始化狀態** 開始）
   ![4](4.gif)
