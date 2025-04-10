---
title: D2L - Chapter 2.4 Calculus + 2.5 Automatic Differentiation
summary:
date: 2025-04-09
authors:
  - admin
tags:
  - machine-learning
---

## 2.4 Calculus

### 2.4.1 Derivatives and Differentiation

### 2.4.2 Visualization Utilities

### 2.4.3 Partial Derivatives and Gradients

When there is no ambiguity, ∇ₓf(**x**) is typically replaced by ∇f(**x**). The following rules come in handy for differentiating multivariate functions:

- For all **A** ∈ ℝᵐˣⁿ we have ∇ₓ(**A**x) = **A**ᵀ and ∇ₓ(**x**ᵀ**A**) = **A**.
- For square matrices **A** ∈ ℝⁿˣⁿ we have that ∇ₓ(**x**ᵀ**A**x) = (**A** + **A**ᵀ)**x** and in particular  
  ∇ₓ‖**x**‖² = ∇ₓ(**x**ᵀ**x**) = 2**x**.

### 2.4.4 Chain Rule

### 2.4.5 Discussion

- **Automatic differentiation** allows us to compute gradients systematically using composition rules, freeing up cognitive effort for higher-level tasks.
- **Gradient computation** for vector-valued functions involves matrix multiplication, following the chain of variable dependencies.
- **Dependency graphs**:
  - Evaluated **forward** to compute function outputs.
  - Traversed **backward** to compute gradients (backpropagation).
- **Backpropagation** is a computational application of the chain rule, introduced formally in later chapters.
- In **optimization**, gradients indicate how to adjust model parameters to reduce the loss.
- Every step of an optimization algorithm relies on computing the gradient.

## 2.5 Automatic Differentiation

### 2.5.1 A simple function

```python
from jax import grad
x = jnp.arange(4.0)
x
y = lambda x: 2 * jnp.dot(x, x)
y(x)
# The `grad` transform returns a Python function that
# computes the gradient of the original function
x_grad = grad(y)(x)
x_grad
```

### 2.5.2 Backward for Non-Scalar Variables

### 2.5.3 Detaching Computation

- move some calculations outside the recorded computational graph

### 2.5.4. Gradients and Python Control Flow

### 2.5.5 Discussion

For now, try to remember these basics:

- (i) attach gradients to those variables with respect to which we desire derivatives;
- (ii) record the computation of the target value;
- (iii) execute the backpropagation function;
- (iv) access the resulting gradient.
