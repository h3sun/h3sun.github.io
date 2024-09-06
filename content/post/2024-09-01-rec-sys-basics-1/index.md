---
title: Recommender System Study Notes Basics (1/1)
summary: Recommender System Basics
date: 2024-09-01
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

## Basics

```markmap {height="200px"}
- Impression
  - Click
    - ScrollToEnd
      - Comment
    - Like
    - Collect
    - Share
```

#### 基础指标

- Click-through rate = Number of clicks / Number of impressions
- Like rate = Number of likes / Number of impressions
- Collection rate = Number of collections / Number of clicks
- Forwarding rate = Number of forwards / Number of clicks
- Completion rate = Number of times scrolled to the end / Number of clicks \* f(Note length)

#### 北极星指标

- 用户规模
  DAU daily active user
  MAU monthly active user
- 消费
  人均使用时长，人均阅读笔记数量
- 发布
  发布渗透率，人均发布量

#### 实验流程

```markmap {height="200px"}
- Offline Test
  - A/B Test
    - experiment rollout
```

#### Chain of Recommender System

```markmap {height="200px"}
- Nomination
  - Pre-Ranking
    - Ranking
      - Re-Ranking
```

- Nomination O(billion) -> O(k)
  - CF
  - Two Tower
  - Subscribed Content Creator
- Pre-Ranking + Ranking O(k) -> O(100)

  - Use a neural network as follow:
  - Input:
    - User Feature Embedding
    - Item Feature Embedding
    - Stats Embedding
  - Output:
    - Click-through rate
    - Like Rate
    - Collection Rate
    - Forwarding Rate
    - Completion Rate
  - Combine them and output a score

- Re-Ranking O(100) -> O(80?) | for diversity
  - Common methods:
  - MMR
  - DPP
  - use pre-set rules to diversify notes
  - insert ads, change sequence based on rules

#### A/B Test

- Basic flow:
  - new GNN, Offline testing gives promising results
  - online A/B test, vaildate
  - tune hyper parameters
- how to put user into buckets randomly
  - hash user id
  - compute metrics for each bucket, DAU, etc
  - experiment rollout
- Overlapping experiment
  - overlapping: nomination, pre-ranking, ranking, re-ranking, user interface, ads
  - each layer: mutually exclusive
  - between layers: orthogonality
- Holdout mechanism
  - 10% as holdout bucket + 90% as overlapping experiment
  - clean holdout and rollout experiment
- Experiment Rollout
- Experiment Reverse

### Pre-Ranking, Ranking(scoring)

### Re-Ranking
