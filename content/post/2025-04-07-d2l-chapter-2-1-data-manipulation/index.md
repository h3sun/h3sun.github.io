---
title: D2L - Chapter 2.1 Data Manipulation
summary:
date: 2025-04-07
authors:
  - admin
tags:
  - machine-learning
---

[colab](https://colab.research.google.com/github/d2l-ai/d2l-jax-colab/blob/master/chapter_preliminaries/ndarray.ipynb)

### 2.1.1 Get Started

```python
import jax
from jax import numpy as jnp
x = jnp.arange(12)
x -> Array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11], dtype=int32)
x.size -> 12
x.shape -> (12, )
X = x.reshape(3, 4)
X
jnp.zeros((2, 3, 4))
jnp.ones((2, 3, 4))
# Any call of a random function in JAX requires a key to be
# specified, feeding the same key to a random function will
# always result in the same sample being generated
jax.random.normal(jax.random.PRNGKey(0), (3, 4))
jnp.array([[2, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
```

#### Personal Note

1. shape -> (axis-0, axis-1, ...)
2. reshape must be multiple

### 2.1.2 Indexing and Slicing

```python
X[-1], X[1:3]
# JAX arrays are immutable. jax.numpy.ndarray.at index
# update operators create a new array with the corresponding
# modifications made
X_new_1 = X.at[1, 2].set(17)
X_new_1
X_new_2 = X_new_1.at[:2, :].set(12)
X_new_2
```

### 2.1.3 Operations

```python
jnp.exp(x)
x = jnp.array([1.0, 2, 4, 8])
y = jnp.array([2, 2, 2, 2])
x + y, x - y, x * y, x / y, x ** y
X = jnp.arange(12, dtype=jnp.float32).reshape((3, 4))
Y = jnp.array([[2.0, 1, 4, 3], [1, 2, 3, 4], [4, 3, 2, 1]])
# axis sets which direction to stack, axis = 0, stacks on the row, meaning will have 6 in axis-0
jnp.concatenate((X, Y), axis=0), jnp.concatenate((X, Y), axis=1)

X == Y
X.sum()
```

#### Personal Note

1. axis sets which direction to stack. For example, axis = 0, stacks on the row, meaning will have 6(3 + 3) items in axis-0.

### 2.1.4 Broadcasting

By now, you know how to perform elementwise binary operations on two tensors of the same shape. Under certain conditions, even when shapes differ, we can still [perform elementwise binary operations by invoking the broadcasting mechanism.] Broadcasting works according to the following two-step procedure: (i) expand one or both arrays by copying elements along axes with length 1 so that after this transformation, the two tensors have the same shape; (ii) perform an elementwise operation on the resulting arrays.

```python
a = jnp.arange(3).reshape((3, 1))
b = jnp.arange(2).reshape((1, 2))
a,
a + b
```

#### Personal Note

1. broadcasting copies data to reach lcm and do computation.

### 2.1.5 Conversion to Other Python Objects

```python
a = jnp.array([3.5])
a
a.item()
float(a) 🙅
int(a) 🙅
```
