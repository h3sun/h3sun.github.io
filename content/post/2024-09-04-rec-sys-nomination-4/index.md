---
title: Recommender System Study Notes - Nomination(4/6)
summary: two tower model full update/incremental update, self-supervised learning
date: 2024-09-04
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

- Two tower model full update/incremental update.
  - Online Nomination: Online compute user ID, ANN in stored vector database with item.
  - Full Update: 1 epoch, 1 day.
  - Incremental Update: Only upadte ID embedding, frozen the fully connected layer.
- Two tower model Self-supervised learning
  - Problem: Two tower cannot learn item with limited amount of clicks (data biased).
  - Self-supervised learning: random feature transformation
  - bi', bi'' high similarity
  - bi', bj'' low similarity
  - Training:
    - pick n (user - item) as a batch for training.
    - m items as a batch(uniform sampling) for self-supervised learning.
    - $\frac{1}{n} \sum_{i=1}^{n} L_{\text{main}}[i] + \alpha \cdot \frac{1}{m} \sum_{j=1}^{m} L_{\text{self}}[j]$

### Online Nomination

#### Implementation

- Item
  - Use neural network to compute item b's embedding.
  - Store them in vectorized databse (Milvus, Faiss, HnswLib).
  - Indexing, for ANN search.
- User
  - Use user ID, feature, etc to compute user a's embedding.
  - ANN search
    - Use a as query.
    - Return the top k as return.
  - Why store item but compute user online?
    - User is only 1, item is O(b).
    - User often changes quickly.

### Model Update

#### Full Update VS Incremental Update

- Full Update
  - Use yesterday's tfrecord, train 1 epoch.
  - Update the new user neural network, store item data into database.
  - Easy to implement, easy requirement for the system.
- Incremental Update(online learning)
  - Why? User's interest can change quickly.
  - Data streaming, TFRecord.
  - Online learning, gradient descent, only update ID Embedding, not other parameters in the neural network.
  - Update the new ID embedding.
  - To be discarded after full update.
- Why only incremental update is not having the best result?
  - Hourly data is biased.
  - Full update: random shuffle, 1 epoch.
  - Incremental update: train from morning to night, this is not having the best result.
  - random shuffle makes a positive impact on the learning.

### Two tower model Self-supervised learning

#### Problem

- The head effect in the recommendation system is severe:

  - A small number of items account for most of the clicks.
  - Most items have a low number of clicks.

- The features of high-click items are well-learned, but the features of long-tail items are not well-learned.
- Self-supervised learning: data augumentation, to better learn long-tail items' embedding.

#### Intuition

- A simple feature transformation will make minimal impact on their feature embeddings, and pairwise, the similarity still remains large.
- i -> i' -> bi', i -> i'' -> bi'', j -> j' -> bj', j -> j''->bj''
- i, bi' and bi'' has high similarity
- i, j, bi' and bj'' has low similarity
- cos(bi, bi'') big, cos(bi', bj') small

#### Methods

1. Random Mask

- Randomly pick labels, and set them to null.

2. Dropout

- Randomly dropout 50% label.

3. Complementary

- ID, Label, Keywords, City.
- Randomly divide them into two {ID, keyword} and {Label, City}.
- {ID, default, Keyword, default} -> Item embedding.
- {default, Label, default, City} -> Item embedding.

4. Mask a set of correlated features

- U = {Male, Female, Unisex}.
- V = {Photography, Football, IT, Makeup}.
- u = female, v = Makeup, p(u, v) is high.
- u = female, v = IT, p(u, v) is low.
- $MI(U, V) = \sum_{u \in U} \sum_{v \in V} p(u, v) \cdot \log \frac{p(u, v)}{p(u) \cdot p(v)}$

Learning:

- Assume there are a total of \( k \) features. Calculate the MI (Mutual Information) between any two features offline, resulting in a \( k \times k \) matrix.
- Randomly select one feature as a seed and find the \( k/2 \) features most related to the seed.
- Mask the seed and its related \( k/2 \) features, retaining the remaining \( k/2 \) features.

Training:

- Sampling m items as a batch
- cos(bi', bi'') -> 1, other cos(bi', bj'') -> 0
