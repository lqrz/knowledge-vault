# Binary Cross-Entropy Loss

Binary cross-entropy is the two-outcome version of cross-entropy. It is used when each prediction is a binary decision, such as positive versus negative, present versus absent, or class 1 versus class 0.

For target $y \in \{0, 1\}$ and predicted probability $\hat{p}$ for the positive class, binary cross-entropy is:

$$
L(y, \hat{p}) = -\left[y\log(\hat{p}) + (1-y)\log(1-\hat{p})\right]
$$

If $y = 1$, the loss becomes:

$$
L = -\log(\hat{p})
$$

If $y = 0$, the loss becomes:

$$
L = -\log(1-\hat{p})
$$

So the loss penalizes low predicted probability for the true binary outcome.

## Intuition

Binary cross-entropy asks: how much probability did the model assign to what actually happened?

- If the label is `1`, the model should make $\hat{p}$ close to `1`.
- If the label is `0`, the model should make $\hat{p}$ close to `0`.

The logarithm makes confident mistakes expensive. Predicting `0.01` when the label is `1` is much worse than predicting `0.4`.

As an information-theory quantity, this is the binary-outcome case of [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]].

## Logits, Sigmoid, and PyTorch

A binary classifier often produces one logit per binary decision:

$$
z \in \mathbb{R}
$$

Sigmoid converts that logit into a probability:

$$
\hat{p} = \sigma(z) = \frac{1}{1 + e^{-z}}
$$

In PyTorch, prefer `nn.BCEWithLogitsLoss`. It combines sigmoid and binary cross-entropy in one numerically stable operation. Do not manually apply `sigmoid` before `BCEWithLogitsLoss`.

```python
import torch
import torch.nn as nn

logits = torch.tensor([2.0, -1.0, 0.3, -0.7], requires_grad=True)
targets = torch.tensor([1.0, 0.0, 1.0, 0.0])

criterion = nn.BCEWithLogitsLoss()
loss = criterion(logits, targets)
loss.backward()
```

Functional form:

```python
import torch.nn.functional as F

loss = F.binary_cross_entropy_with_logits(logits, targets)
```

For a model:

```python
model = nn.Linear(10, 1)
criterion = nn.BCEWithLogitsLoss()

x = torch.randn(4, 10)
targets = torch.tensor([[1.0], [0.0], [1.0], [0.0]])

logits = model(x)
loss = criterion(logits, targets)
loss.backward()
```

For inference, apply sigmoid after training to interpret the logit as a probability:

```python
probs = torch.sigmoid(logits)
predictions = probs >= 0.5
```

The `0.5` threshold is conventional, not mandatory. It can be tuned for precision, recall, cost, or class imbalance.

## Multi-Label Classification

Binary cross-entropy also works for multi-label classification, where each example can belong to multiple classes independently.

In that case, the model emits one logit per label:

```python
import torch
import torch.nn.functional as F

batch_size = 3
num_labels = 5

logits = torch.randn(batch_size, num_labels, requires_grad=True)
targets = torch.tensor([
    [1.0, 0.0, 1.0, 0.0, 0.0],
    [0.0, 1.0, 0.0, 0.0, 1.0],
    [1.0, 1.0, 0.0, 0.0, 0.0],
])

loss = F.binary_cross_entropy_with_logits(logits, targets)
loss.backward()
```

Use this when labels are independent yes/no decisions. Do not use softmax cross-entropy for this case, because softmax forces probability mass to be shared across mutually exclusive classes.

## `BCELoss` Versus `BCEWithLogitsLoss`

`nn.BCELoss` expects probabilities, so it should receive values after sigmoid:

```python
probs = torch.sigmoid(logits)
loss = nn.BCELoss()(probs, targets)
```

This is usually less stable than `BCEWithLogitsLoss`, especially for very large positive or negative logits. Prefer:

```python
loss = nn.BCEWithLogitsLoss()(logits, targets)
```

## Class Imbalance

For imbalanced binary data, `BCEWithLogitsLoss` supports `pos_weight`, which increases the relative weight of positive examples.

```python
criterion = nn.BCEWithLogitsLoss(pos_weight=torch.tensor([3.0]))
loss = criterion(logits, targets)
```

This can improve recall for rare positives, but it changes the objective. The resulting logits and probabilities may need calibration or threshold tuning.

## Limitations and Caveats

Binary cross-entropy assumes binary targets or soft targets in `[0, 1]`. Targets outside that range are not meaningful for the probability interpretation.

It assumes each binary output is modeled independently. For mutually exclusive classes, use [[cross-entropy-loss]] instead.

It **penalizes confidence, not just correctness**. A correct but uncertain prediction can have higher loss than a confident correct prediction.

It can become misleading under severe label noise because it keeps pushing probability toward the observed label even when the label is wrong.

A low binary cross-entropy does not choose a deployment threshold for you. Threshold choice depends on the cost of false positives and false negatives.

## Related

- [[loss-functions]]
- [[cross-entropy-loss]]
- [[../activations/sigmoid-activation|sigmoid-activation]]
- [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]]
- [[topics/statistics/wiki/information-theory/entropy|entropy]]
- [[topics/machine-learning/wiki/empirical-risk-minimisation|empirical-risk-minimisation]]

## Sources

- [PyTorch Documentation: `torch.nn.BCEWithLogitsLoss`](https://docs.pytorch.org/docs/stable/generated/torch.nn.BCEWithLogitsLoss.html)
- [PyTorch Documentation: `torch.nn.BCELoss`](https://docs.pytorch.org/docs/2.9/generated/torch.nn.BCELoss.html)
- [PyTorch Documentation: `torch.nn.functional.binary_cross_entropy_with_logits`](https://docs.pytorch.org/docs/stable/generated/torch.nn.functional.binary_cross_entropy_with_logits.html)
- [PyTorch Documentation: `torch.nn.functional.binary_cross_entropy`](https://docs.pytorch.org/docs/stable/generated/torch.nn.functional.binary_cross_entropy.html)
- [MIT 6.390 Intro to Machine Learning: Neural Networks](https://introml.mit.edu/notes/neural_networks.html)
