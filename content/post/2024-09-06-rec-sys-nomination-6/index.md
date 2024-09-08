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
