---
title: D2L - Chapter 2.2 Data Preprocessing + 2.3 Linear Algebra
summary:
date: 2025-04-08
authors:
  - admin
tags:
  - machine-learning
---

## 2.2 Data Preprocessing

### 2.2.1 Reading the Dataset

### 2.2.2 Data Preparation

```python
inputs = pd.get_dummies(inputs, dummy_na=True)
inputs = inputs.fillna(inputs.mean())
```

### 2.2.3 Conversion To the Tensor Format

```python
from jax import numpy as jnp

X = jnp.array(inputs.to_numpy(dtype=float))
y = jnp.array(targets.to_numpy(dtype=float))
X, y
```

## 2.3 Linear Algebra

### 2.3.1 Scalars

```pyhton
x = jnp.array(3.0)
y = jnp.array(2.0)
```

### 2.3.2 Vectors

```pyhton
x = jnp.arange(3)
x
x[2]
len(x)
x.shape
```

### 2.3.3 Matrices

```python
A = jnp.arange(6).reshape(3, 2)
A
A.T
A = jnp.array([[1, 2, 3], [2, 0, 4], [3, 4, 5]])
A == A.T
```

### 2.3.4 Tensors

```python
jnp.arange(24).reshape(2, 3, 4)
```

### 2.3.5 Basic Properties of Tensor Arithmetic

1. element-wise addition
2. element-wise multiplication (Hadamard Product)
3. element \* scalar

### 2.3.6 Reduction

By default, invoking the sum function reduces a tensor along all of its axes, eventually producing a scalar. Our libraries also allow us to [specify the axes along which the tensor should be reduced.] To sum over all elements along the rows (axis 0), we specify axis=0 in sum. Since the input matrix reduces along axis 0 to generate the output vector, this axis is missing from the shape of the output.

```python
A.shape, A.sum(axis=0).shape
C = jnp.arange(24).reshape(2, 3, 4)
C
C.sum(axis=2)
```

### 2.3.7 Non-Reduction Sum

```
print(A)
sum_A = A.sum(axis=1, keepdims=True)
sum_A, sum_A.shape
[[0. 1. 2.]
 [3. 4. 5.]]
(Array([[ 3.],
        [12.]], dtype=float32),
 (2, 1))

A.cumsum(axis=0)
```

### 2.3.8 Dot Products

weighted sum/ weighted average...

```python
y = jnp.ones(3, dtype = jnp.float32)
x, y, jnp.dot(x, y)
```

### 2.3.9 Matrix-Vector Products

### 2.3.10 Matrix-Matrix Products

n*k X k*m
-> n \* m dot products

### 2.3.11 Norms

```
u = jnp.array([3.0, -4.0])
jnp.linalg.norm(u)
jnp.linalg.norm(u, ord=1) # same as jnp.abs(u).sum()
jnp.linalg.norm(jnp.ones((4, 9)))
```

### 2.3.12 Discussions

- Scalars, vectors, matrices, and tensors are the basic mathematical objects used in linear algebra and have zero, one, two, and an arbitrary number of axes, respectively.
- Tensors can be sliced or reduced along specified axes via indexing, or operations such as sum and mean, respectively.
- Elementwise products are called Hadamard products. By contrast, dot products, matrix–vector products, and matrix–matrix products are not elementwise operations and in general return objects having shapes that are different from the the operands.
- Compared to Hadamard products, matrix–matrix products take considerably longer to compute (cubic rather than quadratic time).
- Norms capture various notions of the magnitude of a vector (or matrix), and are commonly applied to the difference of two vectors to measure their distance apart.
- Common vector norms include the l1 and l2 norms, and common matrix norms include the spectral and Frobenius norms.
