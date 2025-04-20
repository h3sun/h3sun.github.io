---
title: D2L - Chapter 3.3 Synthetic Regression Data
summary: d2l.ai, chapter 3.3 Synthetic Regression Data
date: 2025-04-19
authors:
  - admin
tags:
  - machine-learning
---

## 3.3 Synthetic Regression Data

### 3.3.1 Generating the Dataset

generate the fake data

```python

class SyntheticRegressionData(d2l.DataModule):  #@save
    """Synthetic data for linear regression."""
    def __init__(self, w, b, noise=0.01, num_train=1000, num_val=1000,
                 batch_size=32):
        super().__init__()
        self.save_hyperparameters()
        n = num_train + num_val
        key = jax.random.PRNGKey(0)
        key1, key2 = jax.random.split(key)
        self.X = jax.random.normal(key1, (n, w.shape[0]))
        noise = jax.random.normal(key2, (n, 1)) * noise
        self.y = jnp.matmul(self.X, w.reshape((-1, 1))) + b + noise
```

### 3.3.2 Reading the Dataset

```python
@d2l.add_to_class(SyntheticRegressionData)
def get_dataloader(self, train):
    if train:
        indices = list(range(0, self.num_train))
        # The examples are read in random order
        random.shuffle(indices)
    else:
        indices = list(range(self.num_train, self.num_train+self.num_val))
    for i in range(0, len(indices), self.batch_size):
        batch_indices = jnp.array(indices[i: i+self.batch_size])
        yield self.X[batch_indices], self.y[batch_indices]

```

### 3.3.3 Concise Implementation of the Data Loader

```python
@d2l.add_to_class(d2l.DataModule)  #@save
def get_tensorloader(self, tensors, train, indices=slice(0, None)):
    tensors = tuple(a[indices] for a in tensors)
    # Use Tensorflow Datasets & Dataloader. JAX or Flax do not provide
    # any dataloading functionality
    shuffle_buffer = tensors[0].shape[0] if train else 1
    return tfds.as_numpy(
        tf.data.Dataset.from_tensor_slices(tensors).shuffle(
            buffer_size=shuffle_buffer).batch(self.batch_size))

@d2l.add_to_class(SyntheticRegressionData)  #@save
def get_dataloader(self, train):
    i = slice(0, self.num_train) if train else slice(self.num_train, None)
    return self.get_tensorloader((self.X, self.y), train, i)
```

### 3.3.4 Summary
