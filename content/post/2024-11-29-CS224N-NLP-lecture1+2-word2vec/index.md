---
title: CS224n-NLP-lecture 1, 2
summary: This article sumarizes lecture 1 and 2, covering word2vec
date: 2024-11-29
authors:
  - admin
tags:
  - machine-learning
image:
  caption: "Image credit: [**Unsplash**](https://unsplash.com)"
---

# Word2Vector

## How

- Represent words by their context (Distributional semantics): A word's meaning is given by words frequently appear close-by, within a fixed-size.
- Use the many contexts of w to build up a representation of w.
- We build a dense vector for each word, chosen so that it is similar to vectors of words that appear in similar context.
- Note that word vectors are also called word embeddings, or neural word representations. They are a distributed representation.

## Word2Vector idea

- We have a large body of text (corpus).
- Every word in a fixed vocabulary is represented by vector.
- Go through each position t in the text, which has a center word (c) and a context word (o).
- Use similarity of word vectors for c and o, calculate the probablity of o given c.
- Keep adjusting the word vector to maximize this probablity.

## Math

- $L(\theta) = \prod_{t=1}^T \prod_{\substack{-m \leq j \leq m \\ j \neq 0}} P(w_{t+j} \mid w_t; \theta)$
- $J(\theta) = -\frac{1}{T} \log L(\theta) = -\frac{1}{T} \sum_{t=1}^T \sum_{\substack{-m \leq j \leq m \\ j \neq 0}} \log P(w_{t+j} \mid w_t; \theta)$
- $P(o \mid c) = \frac{\exp(u_o^T v_c)}{\sum_{w \in V} \exp(u_w^T v_c)}$
  - Exponentiation makes anything positive
  - Dot product compares similarity of o and c.
  - Normalize over entire vocabulary to give probability distribution

## Gradient Descent and Stochastic Gradient Descent

### Problem:

- J is a function of all windows in the corpus, gradient is expensive to compute.

### Solution:

- SGD Stochastic Gradient Descent
- SGD in each window: we only have at most 2m + 1 words
- Either we need sparse matrix updates operations to only update certain rows of full embedding metrics U and V, or we need to keep around a hash of word vector. Millions of word vectors allow us to do distributed computing.

## Word2Vector Algo Family

### Two models:

- Skip-gram(SG), predict context (outside) words given center words.
- Continuous bag of words (CBOW), predict context words from context words.

### Negative Sampling

#### Problem

- The normalization demonitator is computionally expensive.

#### Solution

- Train a binary logistic regression for a true pair vs noise pair.
- $J(\theta) = \frac{1}{T} \sum_{t=1}^{T} J_t(\theta)$
- $J_t(\theta) = \log \sigma \left( u_o^\top v_c \right) + \sum_{i=1}^{k} \mathbb{E}_{j \sim P(w)} \left[ \log \sigma \left( -u_j^\top v_c \right) \right]$
- $\sigma(x) = \frac{1}{1 + e^{-x}}$

1. Take k negative samples
2. Maxmize probablity that real words appear, minimize probablity that random words appear around the center words.
3. Sample with P(w) = u(w)^(3/4)

## Co-occurence counting

### Problems:

1. vector increase in sizes
2. Subsequent classification models have sparsity issues.

### Singular Value Decomposition

X = USV

### GloVe

Crucial Insight: Ratios of co-occurence probablities can encode meaning components (meaning component means going from male to female etc).

$J = \sum_{i,j=1}^{V} f(X_{ij}) \left( w_i^\top \tilde{w}_j + b_i + \tilde{b}_j - \log X_{ij} \right)^2$

- $X\_{ij}$: The co-occurrence count between words \( i \) and \( j \).
- \( w_i \): The word vector for the word \( i \).
- \( \tilde{w}\_j \): The context word vector for the word \( j \).
- \( b_i \): The bias term for the word \( i \).
- \( \tilde{b}\_j \): The bias term for the context word \( j \).
- \( \log X*{ij} \): The logarithm of the co-occurrence count \( X*{ij} \).
- \( f(X\_{ij}) \): A weighting function to limit the influence of very frequent or very infrequent word pairs.
- \( x\_{\text{max}} \): A maximum co-occurrence count threshold.

## How to evaluate

### Intrinsics

#### Definition

1. Evaluation on a specific subtask.
2. Fast to compute.
3. Helps to understand the system.
4. Not clear if it really helps unless correlation to real task is established.

#### Example

1. Word Vector Analogy

- man:woman :: king:?
- Evaluate word vectors by how will their cosine distance after addition captures intuitive semantics and syntatic analogy questions.
- Discard the input words from search

2. WordSim human judgement

### Extrinsics

#### Definition

1. Evaluation on a real task.
2. Can take a long time to compute.
3. Unclear if the subsystem is the probelm or its interaction or other subsystems.
4. If replacing exactly one subsystem is making progress, that's good.

#### Example

Named Entity recognition.

## Word sense

A word can have multiple meanings.
