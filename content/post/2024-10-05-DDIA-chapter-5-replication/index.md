---
title: DDIA Chapter 5 - Repliation
summary: This summarizes the DDIA chapter 5.
date: 2024-10-05
authors:
  - admin
tags:
  - system-design
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

# Importance of Replication

- Fault Tolerance: keep the system running, even when one machine (or several machines) go down.
- Disconnected operation: Allow an application to continue to work when there is a network interruption.
- Latency: Placing data geographically close to users, so users can interact with it with a lower latency.
- Scalability: Being able to handle higher volume of reads than a single machine could handle.

# Three main approaches to replication

1. Single-leader replication: Clients send write operation to a single node(leader), which sends a stream of data change to other replicas(followers). Reads can be performed on any replica, but reads from followers might be stale.
2. Multi-leader replication: Clients send each write to onoe of several leader nodes, any of which can accept writes. The leaders send streams of data change events to each other and any follower nodes.
3. Leaderless replication: Clients send each write to several nodes, and read from several nodes in parallel in order to detect and correct nodes with stale data. w + r > n.

# Three interesting effects can be caused by replication lag

1. Read-after-write consistency

Users should always see data that they submit themselves.

Solution:

- When read something that user can modify, read from leader.
- Track update time t, and from t to t + 1 minutes, read from leader(essentially wait for replica picking up the data from leader).
- Track the logical timestamp and compare it with follower node, if smaller, than find another node.

2. Monotonic reads

After users have seen the data at one point in time, they shouldn't later see the data from ealier point in time.

Solution:

- Always read from a single replica.

3. Consistent prefix reads

Users should see data in a state that makes causal sense: for example, an answer should come after a question.

Solution:

- Write to the same partition.

Finally, we discussed the concurrency issues that are inherent in multi-leader and leaderless replication approaches: because such systems allow multiple writes to happen concurrently, conflicts may occur. We examined an algorithm that a database might use to determine whether one operation happened before another, or whether they happened concurrently. We also touched on methods for resolving conflicts by merging together concurrently written values.
In the next chapter we will continue looking at data that is distributed across multiple machines, through the counterpart of replication: splitting a large dataset into partitions.

From DDIA chapter 5.
