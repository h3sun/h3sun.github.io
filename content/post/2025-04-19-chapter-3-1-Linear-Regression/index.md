---
title: D2L - Chapter 3.1 Linear Regression
summary: d2l.ai, chapter 3.1 Linear Regression
date: 2025-04-19
authors:
  - admin
tags:
  - machine-learning
---

## 3.1 Linear Regression

### 3.1.1 Basics

#### 3.1.1.1 Model

1. y = wx + b
2. search for the best w and b
3. measure of the quality of some given model (todo - 1)
4. a procedure for updating the model to improve its quality (todo - 2)

#### 3.1.1.2 Loss Function

$$
L(\mathbf{w}, b)
      = \frac{1}{n} \sum_{i=1}^{n} l^{(i)}(\mathbf{w}, b)
      = \frac{1}{n} \sum_{i=1}^{n} \frac{1}{2}\!\left(\mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)}\right)^{2}. \tag{3.1.6}
$$

#### 3.1.1.3 Analytic Solution

$$
\partial_{\mathbf{w}} \| \mathbf{y} - \mathbf{Xw} \|^2 = 2 \mathbf{X}^\top (\mathbf{Xw} - \mathbf{y}) = 0
\quad \text{and hence} \quad
\mathbf{X}^\top \mathbf{y} = \mathbf{X}^\top \mathbf{Xw}. \tag{3.1.8}
$$

Solving for $\mathbf{w}$ provides us with the optimal solution for the optimization problem. Note that this solution

$$
\mathbf{w}^* = (\mathbf{X}^\top \mathbf{X})^{-1} \mathbf{X}^\top \mathbf{y} \tag{3.1.9}
$$

#### 3.1.1.4 Minibatch Stochastic Gradient Descent

1. Gradient Descent
2. Stochastic Gradient Descent (SGD)
3. MiniBatch SGD
   $$
   \mathbf{w} \leftarrow \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}_t} \partial_{\mathbf{w}} l^{(i)}(\mathbf{w}, b)
   = \mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}_t} \mathbf{x}^{(i)} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
   \tag{3.1.11}
   $$

$$
b \leftarrow b - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}_t} \partial_b l^{(i)}(\mathbf{w}, b)
= b - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}_t} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right).
$$

#### 3.1.1.5 Predictions

Plug w and b to predict y

### 3.1.2 vectorization for Speed

```python

jax
tensorflow
n = 10000
a = jnp.ones(n)
b = jnp.ones(n)
# JAX arrays are immutable, meaning that once created their contents
# cannot be changed. For updating individual elements, JAX provides
# an indexed update syntax that returns an updated copy
c = jnp.zeros(n)
t = time.time()
for i in range(n):
    c = c.at[i].set(a[i] + b[i])
f'{time.time() - t:.5f} sec'
/* 22s */

t = time.time()
d = a + b
f'{time.time() - t:.5f} sec'
/* 0.017s */
```

### 3.1.3 The Normal Distribution and Squared Loss

### 3.1.4 Linear Regression as a Neural Network

#### 3.1.4.1 Biology

### 3.1.5 Summary

introduced/reviewed linear regression, the cornerstone of deep learning.
