---
title: CS224n-NLP-lecture 1
summary: This article sumarizes the cs224n study notes
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
