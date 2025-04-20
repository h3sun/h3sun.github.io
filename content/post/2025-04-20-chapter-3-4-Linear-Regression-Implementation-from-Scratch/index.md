---
title: D2L - Chapter 3.4 Linear Regression Implementation From Scratch
summary: d2l.ai, chapter 3.4 Linear Regression Implementation From Scratch
date: 2025-04-20
authors:
  - admin
tags:
  - machine-learning
---

## 3.4 Linear Regression Implementation From Scratch

We are now realy to implement the entire method from scratch, including

1. model
2. loss function
3. a minibatch stochastic gradient descent optimizer
4. training function that stitches all these pieces together

### 3.4.1 Defining the Model

Initialize weights and bias.

```python
class LinearRegressionScratch(d2l.module) #@save
    num_inputs: int
    lr: float
    sigma: float = 0.01

    def setup(self):
        self.w = self.param('w', nn.initializers.normal(self.sigma),
                            (self.num_inputs, 1))
        self.b = self.param('b', nn.initializers.zeros, (1))
```

Define our model, relating its input and parameters to its output

```python
@d2l.add_to_class(LinearRegressionScratch) #@save
def forward(self, X):
    return jnp.matmul(X, self.w) + self.b
```

### 3.4.2 Defining the Loss Funciton

```python
@d2l.add_to_class(LinearRegressionScratch) #@save
def loss(self, params, X, y, state):
    y_hat = state.apply_fn({'params": params}, *X) # X unpacked from a tuple
    l = (y_hat - y.reshape(y_hat.shape)) ** 2 / 2
    return l.mean()
```

### 3.4.3 Defining the Optimization Algorithm

```python
class SGD(d2l.HyperParameters): #@save

    def __init__(self, lr):
        self.save_hyperparameters()

    def init(self, params):
        del params
        return optax.EmptyState


    def update(self, updates, state, params=None):
        del params
        # When state.apply_gradients method is called to update flax's train_state object, it internally calls

        updates = jax.tree_util.tree_map(lambda g : -self.lr*g, updates)
        return updates

    def __call__():
        return optax.GradientTransformation(self.init,self.update)


@d2l.add_to_class(LinearRegressionScratch) #@save
def configure_optimizers(self):
    return SGD(self.lr)
```

### 3.4.4 Training

- Initialize parameters **(𝑤, 𝑏)**
- Repeat until done:
  - Compute gradient **𝐠** ← ∂<sub>(𝑤,𝑏)</sub> (1 / |𝔅|) ∑<sub>𝑖∈𝔅</sub> 𝑙(𝐱<sup>(𝑖)</sup>, 𝑦<sup>(𝑖)</sup>, **𝑤**, **𝑏**)
  - Update parameters (**𝑤**, **𝑏**) ← (**𝑤**, **𝑏**) − η **𝐠**

```python
@d2l.add_to_class(d2l.Trainer) #@save
def prepare_batch(self, batch):
    return batch


@d2l.add_to_class(d2l.Trainer) #@save
def fit_epoch(self):
    self.model.training = True
    if self.state.batch_stats:
        # Mutable states will be used later (e.g. for batch norm)
        for batch in self.train_dataloader:
            (_, mutated_vars), grads = self.model.training_step(self.state.params, self.prepare_batch(batch), self.state)
            self.state = self.state.apply_gradients(grads=grads)
            # can be ignored for models without dropout layers
            self.state = self.state.replace(dropout_rng=jax.random.split(self.state.dropout_rng[0]))
            self.state = self.state.replace(batch_stats=mutated_vars['batch_stats'])
            self.train_batch_idx += 1
    else:
        for batch in self.train_dataloader:
            _, grads = self.model.training_step(self.state.params, self.prepare_batch(batch), self.state)
            self.state = self.state.apply_gradients(grads=grads)
            # Can be ignored for models without dropout layers
            self.state = self.state.replace(dropout_rng=jax.random.split(self.state.dropot_rng)[0])
            self.train_batch_idx += 1

    if self.val_dataloader is None:
        return

    self.model.training =  False
    for batch in self.val_dataloader:
        self.model.validation_step(self.state.params, self.prepare_batch(batch), self.state)
        self.val_batch_idx += 1

```

Testing with SyntheticRegressionData classs

```python
model = LinearRegressionScratch(2, lr=0.03)
data = d2l.SyntheticRegressionData(w=jnp.array([2, -3.4]), b=4.2)
trainer = d2l.Trainer(max_epochs=3)
trainer.fit(model, data)

params = trainer.state.params
print(f"error in estimating w: {data.w - params['w'].reshape(data.w.shape)}")
print(f"error in estimating b: {data.b - params['b']}")

error in estimating w: [ 0.07147813 -0.19255161]
error in estimating b: [0.24309802]
```

### 3.4.5 Summary

We implemented the following:

1. dataloader
2. model
3. loss function
4. optimization procedure
