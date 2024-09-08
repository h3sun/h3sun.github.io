---
title: Recommender System Study Notes - Nomination(6/6)
summary: GeoHash Nomination, Author Nomination, Cache Nomination, Bloom Filter
date: 2024-09-06
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

- GeoHash Nomination
- Author Nomination
  - Subscribed Author
  - Interacted Author
  - Similar Author
- Cache Nomination

### GeoHash Nomination

- Indexing
  - GeoHash -> Item with good quality(reverse chronological order)
- No personalized

### Subscribed Author Nomination

- Indexing:
  - User -> subscribed authors
  - author -> uploaded items
- Nomination:
  - User -> Subscribed Author -> New Item

### Interacted Author Nomination

- If someone likes/love/forward an item
- Indexing:
  - User -> Interacted Author (update it)
- Nomination:
  - User -> Interacted Author -> New Item

### Similar Author Nomination

- Indexing:
  - Author -> Similar Author
- Nomination:
  - User ID -> Liked Author -> Similar Author -> New Item

### Cache Nomination

- Intuition: Use the previous ranking winning candidates discarded by re-ranking.
- Top 50 items, but get discarded by re-ranking, go to nomination.
- Cache is fixed. How to clear the cache
  - Got impressed
  - At most 10 times
  - At most 3 days

### Bloom Filter

#### Intuition

- If user has seen an item, then we do not recommend this item to user
- For each user, record item that has been recommended(maybe for a month)
- For each item, check if it's been recommended before

#### Basics

- The Bloom filter determines whether an item ID is in the set of exposed items.
- If the judgment is "no", then the item is definitely not in the set.
- If the judgment is "yes", then the item is likely in the set. (There may be false positives, where an unexposed item is incorrectly judged as exposed and thus filtered out.)

####

- The size of the exposed item set is \( n \), the dimensionality of the binary vector is \( m \), and \( k \) hash functions are used.

- The false positive probability of the Bloom filter \( \delta \) is approximately:

  \[
  \delta \approx \left( 1 - \exp\left( \frac{-kn}{m} \right) \right)^k
  \]

- As \( n \) increases, the more 1's there are in the vector, and the higher the false positive probability. (The probability that all \( k \) positions corresponding to unexposed items are set to 1 increases.)

- As \( m \) increases, the vector becomes longer, making hash collisions less likely.

- If \( k \) is too large or too small, it is not ideal; \( k \) has an optimal value.

```
Exposed Filtering Path:

               +-------------+              +---------+              +-------------+
               |   Recall 1  |------------> | Sorting |------------> |   Item 1    |
               +-------------+              +---------+              +-------------+
               |   Recall 2  |                                                   |
               +-------------+                                                   |
               |   Recall 3  |                                                   |
               +-------------+                                                   v
                                                                           +-------------+
                                                                           |   Item 2    |
                                                                           +-------------+
                                                                           |     ...     |
                                                                           +-------------+
                                                                           |   Item q    |
                                                                           +-------------+
                      ^                                                        |
                      |                                                        |
+------------------+  |                                                        |
| Binary Vector    |<-+                                                        |
+------------------+                                                           v
                             +-------------------+     +-----------------------+
                             | Bloom Filter      |<----| Real-time Processing   |
                             | (Exposure Filter) |     |  (Kafka + Flink)       |
                             +-------------------+     +-----------------------+

```
