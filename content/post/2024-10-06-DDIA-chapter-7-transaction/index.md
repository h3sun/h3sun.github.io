---
title: DDIA Chapter 7 - Transaction
summary: This summarizes the DDIA chapter 7.
date: 2024-10-07
authors:
  - admin
tags:
  - system-design
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

# ACID

- Atomicity: All or nothing.
- Consistency: Invariants must always be true (This is from application perspective).
- Isolation: Transactions are executed in a way that their operations do not interfere with each other.
- Durability: Once commited, data will not be forgetten.

# Weak Isolation Levels

## Read Committed (To combat dirty read)

### What is dirty read?

- Read a data that has not been committed by a transaction.

### Implement Read Committed

- Lock on write.
- Lock on read (performance hit).
- Remember old + new value.

## Snapshot Isolation and Repeatable Read (To combat nonrepeatable read)

### What is nonrepetabale read?

- Alice read account A (50).
- Alice executes A = A + 10.
- Alice executes B = B - 10.
- Alice read account B (40).
- Sum is 50 + 40 which is not 100.

### Snapshot Isoltion

- Each transaction sees the snapshot of the database at the start of the transaction.
- MVCC(Multi-version concurrency control).

## Preventing Lost Updates

- Atomic write operations.
- Explicit locking.
- Automotically deleting lost updates.
- Compare and set.
- Conflict resolution and replica.

# Serializability (To combat phantom)

## Write Skew and Phantoms

oncall-rotation in emergency room.

- Pattern

1. select
2. decide how to continue based on select
3. INSERT, UPDATE, DELTE

## Actual Serial Execution

- Every transaction must be small and fast for performance.
- It is limited to use cases where the active dataset can fit in memory for performance.
- Write throughput must be low enough to be handled on a single CPU core, or else transactions need to be partitioned without requiring cross-partition coordination.
- Cross-partition transactions are possible, but there is a hard limit to the extent to which they can be used.

## Two-Phase Locking

- read: acquire the lock in shared mode, if exclusive exists, wait.
- write: acquire the lock in exclusive mode.
- read then write: acquire in shared first, then exclusive.
- commit or abort: hold until abort or commit

## Serializable Snapshot Isolation (SSI)

- Goal of SSI: full serializability with small perf penalty compared to snapshot isolation.
- Used in single-node (PostgreSQL 9.1) and distributed, may become the new default.

### Pessimistic versus optimistic concurrency control

- 2PL is pessimistic
- SSI is optimistic: When a transaction wants to commit, the database checks isolation violations
- If there is enough spare capacity, and if contention between transactions is not too high, optimistic concurrency control techniques perform better
- On top of snapshot isolation, SSI adds an algorithm for detecting serialization conflicts among writes and determining which transactions to abort.

Decisions based on an outdated premise
A premise is a fact that was true at the beginning of the transaction. How does the database know if a query result might have changed?

### Detecting stale MVCC reads

- The database needs to track when a transaction ignores another transaction’s writes due to MVCC visibility rules.
- Check any of the ignored writes has been committed and abort if so.
- Wait until committing because read-only won't write skew, and other write transactions might be aborted.
- Key is to avoid unnecessary aborts.

### Detecting writes that affect prior reads

- DB track ephemeral information about transactions reading data on index, or at the table level.
- When a transaction writes, notify existing reader of that data
- Check other writes commit when commit reader.
- (Question: why not notify existing reader when committing the other writer transaction?)

### Performance of serializable snapshot isolation

- The rate of aborts significantly affects the overall performance of SSI. So SSI requires short read-write transactions, but it's less sensitive to slow transactions than two-phase locking or serial execution.

source:

- https://xgwang.me/posts/ddia-7-transactions/
- ddia
