---
title: Recommender System Study Notes - Nomination(1/)
summary: Item CF, Swing, User CF
date: 2024-09-01
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

- Item CF
- Swing
- User CF

### Item CF

#### Intuition

- If user likes i1 and i1 is similar with i2 -> Then user is likely to like i2

#### Formula

- $score = \sum_j \text{like}(\text{user}, \text{item}\_j) \times \text{sim}(\text{item}\_j, \text{item})$, to compute Item CF score
- $\text{sim}(i_1, i_2) = \frac{|\mathbf{v}|}{\sqrt{|\mathbf{w}_1| \cdot |\mathbf{w}_2|}}$, to compute similarity, w1 as the user who likes i1, w2 as the set, v as the intersection between i1 and i2.
- $\text{sim}(i*1, i_2) = \frac{\sum*{v \in v} \text{like}(v, i*1) \cdot \text{like}(v, i_2)}{\sqrt{\sum*{u*1 \in w_1} \text{like}^2(u_1, i_1)} \cdot \sqrt{\sum*{u_2 \in w_2} \text{like}^2(u_2, i_2)}}$, to take like score into consideration).

#### Implementation

##### Offline:

- User -> Item Indexing
  - return the last-n items with interaction that this user interacts with.
- Item -> Item Indexing
  - compute sim (i, j) maybe maintaining User->Item
  - for each item, find the cloest k items

##### Online Inference: (user id -> 100 items)

- user id -> last-n items
- for each item in last-n, use item-item to find k items
- Use score formula to compute n \* k items' score
- pick top 100, and output top 100 items as the nomination result in this Item CF Channel

### Swing

#### Intuition

- If two items are not that relavant, but it got forwarded into a same small group, we want to lower the similarity score.

#### Formula

- u1 -> j1, u2 -> j2
  - $\text{overlap}(u_1, u_2) = |\mathcal{J}_1 \cap \mathcal{J}_2|$
- i1 -> w1, i2 -> w2, v = w1 ∩ w2
  - $\text{sim}(i_1, i_2) = \sum_{u_1 \in V} \sum_{u_2 \in V} \frac{1}{\alpha + \text{overlap}(u_1, u_2)}$, to compute similarity, a is a hyper-parameter
- if overlap is big, then the contribution to similarity becomes low

### User CF

#### Intuition

- I will like the notes which liked by someone who is similar to me.

#### Formula

- Method1: like, collect, click notes are similar
- Method2: Subscribed authors are similar
- u1 -> j1, u2 -> j2, I = j1 ∩ j2, nl: the number of users who likes l
  - $sim = \text{sim}(u_1, u_2) = \frac{|I|}{\sqrt{|\mathcal{J}_1| \cdot |\mathcal{J}_2|}}$
  - $sim = \text{sim}(u_1, u_2) = \frac{\sum_{I \in I} \frac{1}{\log(1 + n_I)}}{\sqrt{|\mathcal{J}_1| \cdot |\mathcal{J}_2|}}$
- $score = \sum_j \text{sim(user, user}_j\text{)} \times \text{like(user}_j, \text{item)}$

#### Implementation

##### Offline:

- User -> Item Indexing
  - return the last-n items with interaction that this user interacts with.
- User -> User Indexing
  - compute sim (i, j)
  - for each user, find the cloest k users

##### Online:

- user-user indexing: id -> k cloest users
- user-item indexing: for each user, find his/her last-n, use user-item to find n items
- compute score: Use the formula to compute n \* k items' score
- rank: pick top 100, and output top 100 items as the nomination result in this User CF Channel
