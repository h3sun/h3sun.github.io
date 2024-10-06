---
title: DDIA Chapter 6 - Partition
summary: This summarizes the DDIA chapter 5.
date: 2024-10-06
authors:
  - admin
tags:
  - system-design
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

## Two types of partition techniques

### Key Range Partition

- How: [a-c], [d-e], ...
- Pros: Efficient for range query.
- Cons: Hot spot.

### Hash Partition

- How: hash first, then use range.
- Pros: Distribute load evenly.
- Cons: Range query is inefficient.

### Hybrid

- How: one part -> partition, the other part -> sort order.

## Partitioning Secondary Indexes

### Partitioning Secondary Indexes by Document

- Database can perform indexing automatically.
- Note: in KV-only DB, application level secondary indexing is prone to inconsistency.
- Each partition maintains its own secondary indexes.
- A.K.A. local index.
- Need to send the query to all partitions, and combine all the results you get back (scatter/gather).
- Read queries are expensive, and prone to latency amplification. But widely used.
- Recommend structure the partitioning scheme so that secondary index queries can be served from a single partition.

### Partitioning Secondary Indexes by Term

- A global index must also be partitioned (term-partitioned).
- Partition the index by the term itself, or using a hash of the term.
- Pro: more efficient read (no scatter/gather).
- Con: write slower and more complicated, require distributed transaction.
- Updates to global secondary indexes are asynchronous, e.g., Amazon DynamoDB.

## Strategies for Rebalancing

### How not to do it: hash mod N

Problem: move data more than necessary

### Fixed number of partitions

- Create many more partitions than there are nodes, and assign several partitions to each node.
- The new node can steal a few partitions from every existing node.
- The number of partitions is usually fixed when the database is first set up and not changed afterward.
- If partitions are very large, rebalancing and recovery from node failures become expensive. But if partitions are too small, they incur too much overhead.

## Request Routing

### Strategy for routing

1. Send requests to all nodes, and if we do not get result, reoute to next node.
2. Implement a routing tier which knows which node has the specific partition.
3. Client knows the partition.

### How does the component make the routing decision

1. Zookeeper which is a service that knows the partition.
2. Gossip protocol.
3. Mannual rebalance.

source:

- https://xgwang.me/posts/ddia-6-partitioning/#intro
- ddia
