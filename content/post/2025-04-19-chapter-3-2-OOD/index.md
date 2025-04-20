---
title: D2L - Chapter 3.2 Object-Oriented Design for Implementation
summary: d2l.ai, chapter 3.2 Object-Oriented Design for Implementation
date: 2025-04-19
authors:
  - admin
tags:
  - machine-learning
---

## 3.2 Object-Oriented Design for Implementation

1. Module contains models, losses, and optimization methods
2. DataModule provides data loaders for training and validation
3. Both classes are combined using Trainer class which allows us to train models on a variety of hardware platforms.

Most code in this book adapts Module and DataModule. We will touch upon the Trainer class only when we discuss GPUs, CPUs, parallel training, and optimization algorithms.

### 3.2.1 Utilities

```python
def add_to_class(Class):  #@save
    """Register functions as methods in created class."""
    def wrapper(obj):
        setattr(Class, obj.__name__, obj)
    return wrapper

class A:
    def __init__(self):
        self.b = 1

a = A()

@add_to_class(A)
def do(self):
    print('Class attribute "b" is', self.b)

a.do()
# Class attribute "b" is 1



class HyperParameters:  #@save
    """The base class of hyperparameters."""
    def save_hyperparameters(self, ignore=[]):
        raise NotImplemented
# Call the fully implemented HyperParameters class saved in d2l
class B(d2l.HyperParameters):
    def __init__(self, a, b, c):
        self.save_hyperparameters(ignore=['c'])
        print('self.a =', self.a, 'self.b =', self.b)
        print('There is no self.c =', not hasattr(self, 'c'))

b = B(a=1, b=2, c=3)

class ProgressBoard(d2l.HyperParameters):  #@save
    """The board that plots data points in animation."""
    def __init__(self, xlabel=None, ylabel=None, xlim=None,
                 ylim=None, xscale='linear', yscale='linear',
                 ls=['-', '--', '-.', ':'], colors=['C0', 'C1', 'C2', 'C3'],
                 fig=None, axes=None, figsize=(3.5, 2.5), display=True):
        self.save_hyperparameters()

    def draw(self, x, y, label, every_n=1):
        raise NotImplemented

board = d2l.ProgressBoard('x')
for x in np.arange(0, 10, 0.1):
    board.draw(x, np.sin(x), 'sin', every_n=2)
    board.draw(x, np.cos(x), 'cos', every_n=10)

```

### 3.2.2 Models

The Module class is the base class of all models that we will implement. At the very least we need three models. The first, **init** stores the learnable parameters, the **training_step** method accepts a data batch to return the loss value, and finally, **configure_optimizers** returns the optimization method, or a list of them, that is used to update the learnable parameters. Optionally, we can define **validation_steps** to report the evaluation measures. Sometimes we put the code for computing the output into a separate of **forward** method to make it more resubale.

With the introduce of dataclasses in Python3.7, classes decorated with @dataclass automatically add magic methods such as **init** and **repr**. The member variables are defined using type annotations. All Flax modules are Python 3.7 dataclasses.

```python
class Module(nn.Module, d2l.HyperParameters): #@save

    """The base class of models"""
    # No need for save_hyperparam when using Python dataclass.
    plot_train_per_epoch: int = field(default=2, init=False)
    plot_valid_per_epoch: int = field(default=1, init=False)
    # Use default_factory to make sure new plots are generated on each run.
    board: ProgressBoard = field(default_factory=lambda: ProgressBoard(), init=False)

    def loss(self, y_hat, y):
        raise NotImplementedError

    # JAX & Flax do not have a forward-method-like syntax. Flax uses setup
    # and built-in __call__ magic methods for forward pass. Adding here
    # for consistency

    def forward(self, X, *args, **kwargs):
        assert hasattr(self, 'net'), 'Neural network is defined'
        return self.net(X, *args, **kwargs)


    def plot(self, key, value, train):
        """Plot a point in animation."""
        assert hasattr(self, 'trainer'), 'Trainer is not inited'
        self.board.xlabel = 'epoch'
        if train:
            x = self.trainer.train_batch_idx / \
                self.trainer.num_train_batches
            n = self.trainer.num_train_batches / \
                self.plot_train_per_epoch
        else:
            x = self.trainer.epoch + 1
            n = self.trainer.num_val_batches / \
                self.plot_valid_per_epoch
        self.board.draw(x, jax.device_put(value, d2l.cpu()),
                        ('train_' if train else 'val_') + key,
                        every_n=int(n))

    def training_step(self, params, batch, state):
        l, grads = jax.value_and_grad(self.loss)(params, batch[:-1],
                                                 batch[-1], state)
        self.plot("loss", l, train=True)
        return l, grads

    def validation_step(self, params, batch, state):
        l = self.loss(params, batch[:-1], batch[-1], state)
        self.plot('loss', l, train=False)

    def apply_init(self, dummy_input, key):
        """To be defined later in :numref:`sec_lazy_init`"""
        raise NotImplementedError

    def configure_optimizers(self):
        raise NotImplementedError


```

### 3.2.3 Data

The Datamodule class is the base class for data. Quite frequently the **init** method is used to prepare the data.

```python
class DataModule(d2l.HyperParameters):  #@save
    """The base class of data."""
    def __init__(self, root='../data'):
        self.save_hyperparameters()

    def get_dataloader(self, train):
        raise NotImplementedError

    def train_dataloader(self):
        return self.get_dataloader(train=True)

    def val_dataloader(self):
        return self.get_dataloader(train=False)
```

### 3.2.4 Training

```python
class Trainer(d2l.HyperParameters):  #@save
    """The base class for training models with data."""
    def __init__(self, max_epochs, num_gpus=0, gradient_clip_val=0):
        self.save_hyperparameters()
        assert num_gpus == 0, 'No GPU support yet'

    def prepare_data(self, data):
        self.train_dataloader = data.train_dataloader()
        self.val_dataloader = data.val_dataloader()
        self.num_train_batches = len(self.train_dataloader)
        self.num_val_batches = (len(self.val_dataloader)
                                if self.val_dataloader is not None else 0)

    def prepare_model(self, model):
        model.trainer = self
        model.board.xlim = [0, self.max_epochs]
        self.model = model

    def fit(self, model, data, key=None):
        self.prepare_data(data)
        self.prepare_model(model)
        self.optim = model.configure_optimizers()

        if key is None:
            root_key = d2l.get_key()
        else:
            root_key = key
        params_key, dropout_key = jax.random.split(root_key)
        key = {'params': params_key, 'dropout': dropout_key}

        dummy_input = next(iter(self.train_dataloader))[:-1]
        variables = model.apply_init(dummy_input, key=key)
        params = variables['params']

        if 'batch_stats' in variables.keys():
            # Here batch_stats will be used later (e.g., for batch norm)
            batch_stats = variables['batch_stats']
        else:
            batch_stats = {}

        # Flax uses optax under the hood for a single state obj TrainState.
        # More will be discussed later in the dropout and batch
        # normalization section
        class TrainState(train_state.TrainState):
            batch_stats: Any
            dropout_rng: jax.random.PRNGKeyArray

        self.state = TrainState.create(apply_fn=model.apply,
                                       params=params,
                                       batch_stats=batch_stats,
                                       dropout_rng=dropout_key,
                                       tx=model.configure_optimizers())
        self.epoch = 0
        self.train_batch_idx = 0
        self.val_batch_idx = 0
        for self.epoch in range(self.max_epochs):
            self.fit_epoch()

    def fit_epoch(self):
        raise NotImplementedError
```

### 3.2.5 Summary

This object-oriented design provides moduality.
