# Cross-Entropy Loss

Cross-entropy loss is a classification loss that penalizes a model when it assigns low probability to the correct target. In deep learning, it is most commonly used for multi-class classification where exactly one class is correct.

For a target distribution $y$ and predicted distribution $\hat{p}$ over classes, cross-entropy is:

$$
H(y, \hat{p}) = -\sum_{c=1}^{C} y_c \log \hat{p}_c
$$

When the target is one-hot, only the correct class contributes:

$$
H(y, \hat{p}) = -\log \hat{p}_{true}
$$

So the loss is small when the model gives high probability to the correct class, and large when it gives low probability to the correct class.

## Intuition

Cross-entropy can be read as a surprise penalty. If the correct class receives probability near 1, the loss is near 0:

$$
-\log(1) = 0
$$

If the correct class receives probability near 0, the loss becomes very large:

$$
-\log(\hat{p}_{true}) \rightarrow \infty \quad \text{as} \quad \hat{p}_{true} \rightarrow 0
$$

This makes confident wrong predictions much more costly than uncertain wrong predictions.

From an information-theory perspective, this is the code length of the observed class under the model's predicted distribution. The broader concept is covered in [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]].

## Logits, Softmax, and PyTorch

Neural networks usually produce logits, not probabilities. A logit is an unconstrained score for a class:

$$
z = [z_1, z_2, \dots, z_C]
$$

Softmax converts logits into probabilities:

$$
\hat{p}_c = \frac{e^{z_c}}{\sum_{j=1}^{C} e^{z_j}}
$$

In PyTorch, `nn.CrossEntropyLoss` expects raw logits and integer class indices. It internally applies the stable equivalent of `log_softmax` plus negative log-likelihood loss. Do not apply `softmax` before passing logits to `CrossEntropyLoss`.

```python
import torch
import torch.nn as nn

batch_size = 4
num_classes = 3

logits = torch.tensor([
    [2.0, 0.5, -1.0],
    [0.1, 1.5, 0.3],
    [-0.5, 0.2, 2.2],
    [1.0, 0.7, 0.1],
], requires_grad=True)

targets = torch.tensor([0, 1, 2, 0])  # class indices, dtype long

criterion = nn.CrossEntropyLoss()
loss = criterion(logits, targets)
loss.backward()
```

Functional form:

```python
import torch.nn.functional as F

loss = F.cross_entropy(logits, targets)
```

For a model:

```python
model = nn.Linear(10, num_classes)
criterion = nn.CrossEntropyLoss()

x = torch.randn(batch_size, 10)
targets = torch.tensor([0, 1, 2, 0])

logits = model(x)
loss = criterion(logits, targets)
loss.backward()
```

## Interpreting Values

For a single example:

- loss near `0` means the model assigned very high probability to the correct class;
- loss around `log(C)` means the model is roughly as uncertain as a uniform guess over `C` classes;
- high loss means the correct class received low probability.

For example, with 10 classes, a uniform guess has loss:

$$
-\log(0.1) \approx 2.30
$$

This interpretation is useful for debugging. If loss is much worse than the uniform baseline early in training, there may be a label, shape, class-index, or output-layer problem.

## Common Uses

- Single-label multi-class classification.
- Language modeling, where each token prediction is a classification over the vocabulary.
- Semantic segmentation, where each pixel is classified into one class.

## Limitations and Caveats

Cross-entropy assumes the target distribution is meaningful. With noisy labels, it can push the model to become confidently wrong.

It rewards calibrated probabilities only if the model class, data, and training process support calibration. Low cross-entropy does not guarantee good calibration under distribution shift.

It can be sensitive to class imbalance. PyTorch supports a class `weight` argument for reweighting classes, but weighting changes the training objective and should be interpreted carefully.

For single-label classification, targets should usually be integer class indices with shape `(N,)`, not one-hot vectors. PyTorch can accept class probabilities in supported shapes, but class-index targets are generally simpler and more efficient.

Cross-entropy is not the same as accuracy. It cares about confidence, while accuracy only cares whether the top prediction is correct.

## Related

- [[loss-functions]]
- [[binary-cross-entropy-loss]]
- [[../activations/activation-functions|activation-functions]]
- [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]]
- [[topics/statistics/wiki/information-theory/entropy|entropy]]
- [[topics/statistics/wiki/information-theory/kl-divergence|kl-divergence]]
- [[topics/machine-learning/wiki/empirical-risk-minimisation|empirical-risk-minimisation]]

## Sources

- [PyTorch Documentation: `torch.nn.CrossEntropyLoss`](https://docs.pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html)
- [PyTorch Documentation: `torch.nn.functional.cross_entropy`](https://docs.pytorch.org/docs/stable/generated/torch.nn.functional.cross_entropy)
- [MIT 6.390 Intro to Machine Learning: Neural Networks](https://introml.mit.edu/notes/neural_networks.html)
- [Deep Learning Book: Chapter 5, Machine Learning Basics](https://www.deeplearningbook.org/contents/ml.html)
