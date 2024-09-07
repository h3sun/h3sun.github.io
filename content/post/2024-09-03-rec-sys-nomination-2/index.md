---
title: Recommender System Study Notes - Nomination(2/)
summary: discrete feature, matrix completion
date: 2024-09-02
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

- Discrete Feature
- Matrix Completion

### Discrete Feature

#### Example

- sex
- nationality
- english words
- item id
- user id

#### How to encode discrete feature -> vectorization

- one-hot encoding
- embedding (\*)
  - size: vector dimension \* number of items
  - How to implement: TensorFlow, PyTorch -> embedding
  - embedding = Parameter matrix \* one-hot encoding

### Matrix Completion

#### Intuition

```
item id -> embedding
                    \
                     - cos(a, b)
                    /
item id -> embedding
```

#### Formula

- dataset = {u, i, y}
- training, u -> au, i -> bi
- optimization: $\min_{A, B} \sum_{\substack{(u, i, y) \in \Omega}} \left( y - \langle a_u, b_i \rangle \right)^2$

#### Pros/Cons

- Cons
  - It only uses ID embedding without item, user characteristics
    - item characteristics: label, key words
    - user characteristics: gender, age, geo, liked label
    - Two tower Model
  - Negative sample is wrong
    - Positive: User interacted, which is good
    - Negative: User did not interact, which is not correct
  - Training
    - dot product is worse than cosine similarity
    - Loss(regression) is worse than cross entropy

#### Implementation

##### Offline:

- Learning A: user
- Learning B: item
- storing A into {user id: embedding} key-value

##### Online:

- user Id -> find a's vector
- nearest neighbour search, return the top 100 b whose <a, b> is max
- Approximate Nearest Neighbor (ANN)
  - pre process the circile into sectors
  - pick a representative vector(indexing) represents the sector
  - find representative vector, then find vector in that sector instead of the whole circle
