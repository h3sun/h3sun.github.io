---
title: D2L - Chapter 2.6 Probablity and Statistics + 2.7  Documentation
summary:
date: 2025-04-12
authors:
  - admin
tags:
  - machine-learning
---

## Probablity and Statistics

### Covariance Matrix, Diagonal & Off-Diagonal Entries

1. **Covariance Matrix**  
   A covariance matrix \(\Sigma\) is a square matrix that shows the covariance between each pair of coordinates in a random vector. It is:

   - **Symmetric**
   - **Positive semidefinite**

2. **Diagonal Entries**

   - Each diagonal entry \(\Sigma\_{ii}\) is the **variance** of the \(i\)-th coordinate, denoted \(\mathrm{Var}(x_i)\).
   - Variance measures how spread out the \(i\)-th coordinate is around its mean.

3. **Off-Diagonal Entries**

   - Each off-diagonal entry \(\Sigma\_{ij}\) (with \(i \neq j\)) is the **covariance** \(\mathrm{Cov}(x_i, x_j)\).
   - Positive covariance means the two coordinates tend to vary in the same direction (above/below their means together), negative covariance indicates they move in opposite directions, and zero covariance indicates no linear co-movement.

4. **Why Symmetric?**
   - Because \(\mathrm{Cov}(x_i, x_j) = \mathrm{Cov}(x_j, x_i)\), the matrix \(\Sigma\) must be symmetric.

## Documentation

```python

print(dir(jax.random))
help(jax.numpy.ones)
```
