---
title: D2L - Introduction
summary:
date: 2025-04-02
authors:
  - admin
tags:
  - machine-learning
---

## 1.0 Introduction

The author lists the difference between traditional software engineering and machine learning. While software engineering focuses on solving problems with rule-based solutions, machine learning(deep learning) tries to solve problems that are ambiguous and the solution can evlove by itself after seeing more examples.

## 1.1 A Motivating Example

The author lists an example of a wake word system where we feed model with data and let the model learn.

## 1.2 Key Components

1. The data that we can learn from.
2. A model of how to transform the data.
3. An objective function that quantifies how well or badly the model is doing.
4. An algorithm to adjust the model's parameters to optimize the objective function.

### 1.2.1 Data

Machine learning relies on data, which consists of examples represented numerically through features (inputs) and, in supervised learning, labels (targets). Examples include images or medical records, which can be structured as fixed-length vectors—but not all data fits this format, especially text or variable-sized inputs. Deep learning models handle such complexity more effectively than traditional methods. However, more data generally leads to better models, especially in deep learning, where large datasets are often essential. Crucially, the quality and representativeness of the data matter just as much as quantity—biased or flawed data can lead to harmful and unfair outcomes, even without malicious intent.

### 1.2.2 Models

Most machine learning involves transforming input data to make predictions. A model refers to the system that performs this transformation—taking data of one type and outputting another, such as predicting emotions from photos or detecting anomalies in sensor readings. While simple models can solve basic problems, more complex challenges often require more powerful tools. Deep learning stands out by using layered models that apply many transformations in sequence, hence the term deep. This approach enables tackling problems that push beyond the limits of classical methods, though traditional techniques still play a role along the way.

### 1.2.3 Objective function

In machine learning, learning means improving performance at a task over time, which requires a clear way to measure improvement. These measurements are called objective functions or loss functions, which we usually define so that lower values are better. Common examples include squared error for regression and error rate for classification. Some losses are hard to optimize directly, so we often use surrogate objectives that are easier to work with.

We train models by minimizing loss on a training dataset, treating the data as fixed and adjusting model parameters. However, strong performance on training data doesn't guarantee success on new, unseen data. To evaluate generalization, we use a separate test dataset. If a model performs well on training data but poorly on test data, it's said to be overfitting—essentially memorizing rather than learning.

### 1.2.4 Optimization Algorithms

ChatGPT said:
Once we have data, a model, and an objective function, the next step is to optimize the model’s parameters to minimize the loss. In deep learning, this is typically done using gradient descent. This algorithm works by computing how a small change in each parameter would affect the loss, then adjusting the parameters in the direction that reduces the loss. Repeating this process iteratively helps the model gradually improve its performance.

## 1.3 Kinds of Machine Learning Problems

### 1.3.1 Supervised Learning

### 1.3.1.1 Regression

1. Question: How many?
2. Loss: squared error

### 1.3.1.2 Classification

1. Question: Which one?
2. Loss: cross-entropy

### 1.3.1.3 Tagging

1. Question: multi-label classification

### 1.3.1.4 Search

1. Question: releance scores

### 1.3.1.5 Recommender System

### 1.3.1.6 Sequence Learning

1. They require a model either to ingest sequences of inputs or to emit sequences of outputs (or both).

2. Cases

- Tagging and Parsing: tag named entity
- Automatic Speech Recognition: challenge is that input is way longer than output
- Text to Speech: This is the inverse of asr.
- Machine Translation: input/output might not align in the same order.
