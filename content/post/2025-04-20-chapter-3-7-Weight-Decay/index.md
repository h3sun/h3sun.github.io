---
title: D2L - Chapter 3.7 Weight Decay
summary: d2l.ai, chapter 3.7 Weight Decay
date: 2025-04-20
authors:
  - admin
tags:
  - machine-learning
---

## 3.7 Weight Decay

### 3.7.1 Norms and Weight Decay

The total loss with weight decay is:

$$
\mathcal{L}_{\text{total}} = \mathcal{L}_{\text{original}} + \lambda \|\mathbf{w}\|^2
$$

Where:

- \( \mathcal{L}\_{\text{original}} \) is the original loss (e.g., cross-entropy)
- \( \lambda \) is the weight decay coefficient
- \( \|\mathbf{w}\|^2 \) is the squared L2 norm of the weights

The weight update rule with weight decay for linear regression is:

$$
\mathbf{w} \leftarrow (1 - \eta \lambda)\mathbf{w} - \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \mathbf{x}^{(i)} \left( \mathbf{w}^\top \mathbf{x}^{(i)} + b - y^{(i)} \right)
$$

### 3.7.2 High-Dimensional Linear Regression

### 3.7.3 Implementation From Scratch

#### 3.7.3.1. Defining Norm Penalty

```python
def l2_penalty(w):
    return (w ** 2).sum() / 2
```

#### 3.7.3.2. Defining the Model

```python

class WeightDecayScratch(d2l.LinearRegressionScratch):
    lambd: int = 0

    def loss(self, params, X, y, state):
        return (super().loss(params, X, y, state) +
                self.lambd * l2_penalty(params['w']))


data = Data(num_train=20, num_val=100, num_inputs=200, batch_size=5)
trainer = d2l.Trainer(max_epochs=10)

def train_scratch(lambd):
    model = WeightDecayScratch(num_inputs=200, lambd=lambd, lr=0.01)
    model.board.yscale='log'
    trainer.fit(model, data)
    print('L2 norm of w:',
          float(l2_penalty(trainer.state.params['w'])))

```

#### 3.7.3.3 Training without Regularization

```python
train_scratch(0)

train_scratch(3)
```

### 3.7.4. Concise Implementation

```python
class WeightDecay(d2l.LinearRegression):
    wd: int = 0

    def configure_optimizers(self):
        # Weight Decay is not available directly within optax.sgd, but
        # optax allows chaining several transformations together
        return optax.chain(optax.additive_weight_decay(self.wd),
                           optax.sgd(self.lr))

model = WeightDecay(wd=3, lr=0.01)
model.board.yscale='log'
trainer.fit(model, data)

print('L2 norm of w:', float(l2_penalty(model.get_w_b(trainer.state)[0])))
```

### 3.7.5. Summary
