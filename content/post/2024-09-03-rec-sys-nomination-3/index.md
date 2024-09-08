---
title: Recommender System Study Notes - Nomination(3/6)
summary: two tower model Architecture + Training, Positive Samples + Negative Samples
date: 2024-09-03
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

- Two Tower Model - Architecture + Training

  - Pointwise Training
    - View nomination as binary classification
    - Sample: cos(a,b) -> +1, negative sample: cos(a, b) -> -1
    - Positive : negative = 1:2 / 1:3
  - Pairwise Training
    - Intuntion: cos(a, b+) > cos(a, b-)
    - Triplet hinge loss = max(0, cos(a, b-) + m - cos(a, b+))
    - Triplet logistic loss = log(1 + exp(σ) \* cos(a, b-) - cos(a, b+))
  - Listwise Training
    - CrossEntropyLoss

- Two Tower Model - Positive Samples + Negative Samples
  - Positive Sample:
    - Impressed and clicked
  - Negative Sample:
  - Easy negative sample:
    - All items
    - In-batch items
  - Hard negative sample:
    - Nominated, but discarded by ranking
  - Mix easy and hard for training dataset
  - Wrong sampling:
    - Impressed, but not clicked (can be used for training ranking but not nomination)

### Two Tower Model(DSSM) - Architecture and Training

#### Two Tower Model Architecture

```
Neural Network
    └── Concatenate
        ├── Embedding Layer (User ID)
        │      └── Embedding (Vector Representation)
        ├── Embedding Layers (User Discrete Feature)
        │      └── Embedding (Vector Representation)
        └── Normalization, Binning, etc. (normalize to 1, put into buckets)
               └── User Continuous Features (Vector Representation)

```

$\cos(a, b) = \frac{a \cdot b}{\|a\|_2 \cdot \|b\|_2}$

```
                            Cosine Similarity:
                                  /    \
                                /        \
                             a            b
                          /                 \
              Neural Network              Neural Network
                /                                \
         Feature Transformation           Feature Transformation
         /                                          \
User ID, Discrete Features,                 Product ID, Discrete Features,
Continuous Features                         Continuous Features

```

#### Two Tower Model Training

- Pointwise (binary classification)
- Pairwise
- Listwise

##### Pointwise Training

- Intuition: View nomination as binary classification
- positive sample: cos(a,b) -> +1, negative sample: cos(a, b) -> -1
- positive : negative = 1:2 / 1:3

##### Pairwise Training

```
                    cos(a, b⁺)          cos(a, b⁻)
                    /      \            /      \
                   /        \          /        \
                b⁺               a                  b⁻
        (Positive Sample)       (User)           (Negative Sample)
                |                  |                    |
        Neural Network      Neural Network     Neural Network
                |                  |                    |
Feature Transformation   Feature Transformation Feature Transformation
                |                  |                    |
    Positive Item Feature     User Features          Negative Item Feature

```

- Intuntion: cos(a, b+) > cos(a, b-)
- triplet hinge loss = max(0, cos(a, b-) + m - cos(a, b+))
- triplet logistic loss = log(1 + exp(σ) \* cos(a, b-) - cos(a, b+))

##### Listwise Training

```
CrossEntropyLoss(y, s) = -log(s⁺)

                       +-----------------------+
                       |    CrossEntropyLoss   |
                       +-----------------------+
                                 |
                                 |
                          +-------------------------------+
                          |  Softmax Activation Function  |
                          +-------------------------------+
                                 |
      +------------+------------+-------------+ ... +------------+
      |            |            |             |     |            |
     s⁺         s⁻_1       s⁻_2         ...   s⁻_n
 (Positive)    (Negative)  (Negative)        (Negative)

    |             |             |                     |
    |             |             |                     |
 cos(a, b⁺)   cos(a, b⁻_1)  cos(a, b⁻_2)   ...  cos(a, b⁻_n)

   |              |             |                     |
(Positive)    (Negative)   (Negative)         (Negative)
 Sample         Sample        Sample            Sample


```

### Two Tower Model(DSSM) - Positive Samples + Negative Samples

#### Positive Samples

- impressed and clicked
- problem: 20% item takes 80% click
- solution:
  - up-sampling: one sample appear multiple times
  - down-sampling: discard samples

#### Negative Samples

##### Simple Negative Sampling: All Items

###### Uniform Sampling: Unfair to unpopular items

- Most positive samples are popular items.
- If uniform sampling produces negative samples, most of the negative samples are unpopular items.
- positive sample: popular items, negative sample: unpopular items, this is unfair.

###### Non-uniform Sampling: The goal is to suppress popular items

- The probability of negative sampling is positively correlated with popularity (clicks).
- **Sampling probability ∝ (clicks) ^ 0.75**

##### Simple Negative Sampling: Negative Sample within a batch

```
user1 - item1
user2 - item2
user3 - item3

...

user_n - item_n
```

- one batch has n positive samples
- one batch has n(n - 1) negative samples

- The probablity that an item appear in a batch is propotional to the number of times it clicked.
- We aim to get it propotional to clicks^0.75.
- offline training cos(a, bi) - log(pi), pi ∝ clicks
- online inference cos(a, bi)

#### Hard Negative Sampling

##### Hard negative samples:

- Items that are ranked and close to the target (relatively hard).
- Items that are ranked far behind the target (extremely hard).

###### Binary classification for positive and negative samples:

- All items (simple): High classification accuracy.
- Items that are ranked and close to the target (relatively hard): Easy to misclassify.
- Items that are ranked far behind the target (extremely hard): Even easier to misclassify.

#### Datasets

- Mix negative samples
- 50% easy negative sample
- 50% hard negative sample
