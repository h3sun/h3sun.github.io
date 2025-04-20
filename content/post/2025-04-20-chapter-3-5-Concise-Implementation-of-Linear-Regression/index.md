---
title: D2L - Chapter 3.5 Concise Implementation of Linear Regression
summary: d2l.ai, chapter 3.5 Concise Implementation of Linear Regression
date: 2025-04-20
authors:
  - admin
tags:
  - machine-learning
---

## 3.5 Concise Implementation of Linear Regression

### 3.5.1 Defining the Model

```python
class LinearRegression(d2l.Module): #@save
    lr: float
    def setup(self):
        self.net = nn.Dense(1, kernel_init=nn.initializers.normal(0.01))
```

```python
@d2l.add_to_class(LinearRegression) #@save
def forward(self, X):
    return self.net(X)
```

### 3.5.2 Defining the Loss Function

```python
@d2l.add_to_class(LinearRegression) #@save
def loss(self, params, X, y, state):
    y_hat = state.apply_fn({'params': params}, *X)
    return optax.l2_loss(y_hat, y).mean()
```

### 3.5.3 Defining the Optimization Algorithm

```python
@d2l.add_to_class(LinearRegression) #@save
def configure_optimizers(self):
    return optax.sgd(self.lr)
```

### 3.5.4 Training

```
model = LinearRegression(lr=0.03)
data = d2l.SyntheticRegressionData(w=jnp.array([2, -3.4]), b=4.2)
trainer = d2l.Trainer(max_epochs=3)
trainer.fit(model, data)

@d2l.add_to_class(LinearRegression)  #@save
def get_w_b(self, state):
    net = state.params['net']
    return net['kernel'], net['bias']

w, b = model.get_w_b(trainer.state)
```
