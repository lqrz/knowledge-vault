# Loss Functions

A loss function turns a model's output into a scalar training signal. During neural network training, optimization uses gradients of this scalar loss to update the network parameters.

Strictly speaking, the loss function is usually not part of the neural network architecture itself. The architecture defines the computation from input to output; the loss defines how training judges that output. They must still be designed together because the loss must be compatible with the output layer and the task.

## Minimal Requirements

A loss function used for ordinary gradient-based neural network training should have these characteristics.

### Scalar-Valued

The loss must reduce the model output, target, and any extra terms into a scalar:

$$
L(\theta) \in \mathbb{R}
$$

Backpropagation starts from this scalar and computes gradients with respect to parameters. Vector-valued intermediate errors are common, but training normally reduces them by summing or averaging across output dimensions and batch examples.

### Connected to the Computation Graph

The loss must depend on the model parameters through the forward computation:

$$
x \rightarrow f_\theta(x) \rightarrow L
$$

If the loss does not depend on a parameter, there is no gradient signal for that parameter. If the loss contains operations outside the differentiable computation graph, gradient-based optimizers cannot use those operations to update the model.

### Differentiable Enough for the Optimizer

For standard backpropagation, the loss needs gradients with respect to the model outputs and, through the chain rule, the parameters:

$$
\frac{\partial L}{\partial \theta}
$$

It does not need to be differentiable everywhere. ReLU-based networks, absolute error, and hinge-style objectives can still be trained because they are differentiable almost everywhere or have useful subgradients. But a loss with hard thresholds, argmax operations, discrete sampling, or flat regions may block or weaken gradient learning unless it is replaced with a differentiable surrogate or handled by another estimator.

### Compatible with the Output Representation

The loss must match what the final layer represents.

- Regression with a linear output commonly uses squared error or absolute-error variants.
- Binary classification often uses a Bernoulli likelihood, implemented as [[binary-cross-entropy-loss|binary cross-entropy]].
- Multi-class classification often uses a categorical distribution, implemented as softmax plus [[cross-entropy-loss|cross-entropy]] or a numerically stable logits-based equivalent.

This compatibility matters because different losses assume different output ranges and meanings. For example, cross-entropy over class probabilities assumes valid probability-like outputs, while squared error on an unconstrained scalar regression output does not.

The information-theory interpretation of cross-entropy is average code length under a mismatched predicted distribution; see [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]] and [[topics/statistics/wiki/information-theory/kl-divergence|kl-divergence]].

### Numerically Stable and Finite

The loss should be finite for the values the model can produce during training. Losses involving logarithms, exponentials, divisions, or probabilities can become unstable when outputs approach invalid boundaries such as zero probabilities.

In practice, implementations often combine output activation and loss into one stable operation, such as a logits-based cross-entropy, rather than computing softmax probabilities and then taking logs manually.

### Aligned With the Learning Goal

The loss should penalize errors that matter for the task. A mathematically valid scalar loss can still be a poor training objective if it rewards behavior that does not match the real goal.

This is why neural networks often use surrogate losses. Classification accuracy is discrete and hard to optimize directly, so classification models usually train with cross-entropy or another smooth objective whose gradients provide a stronger learning signal.

### Aggregatable Over Examples

Neural networks are usually trained by minimizing an empirical average over examples:

$$
\hat{R}(\theta) = \frac{1}{n}\sum_{i=1}^{n} L(f_\theta(x_i), y_i)
$$

For minibatch training, the loss should produce a useful estimate of this average from a subset of examples. This connects neural network losses to [[topics/machine-learning/wiki/empirical-risk-minimisation|empirical risk minimisation]].

## Useful but Not Strictly Required

These properties are often helpful, but they are not absolute requirements.

### Non-Negativity

Many common losses are non-negative, which makes them easier to interpret. But optimization only depends on relative values and gradients. Adding a constant to a loss changes its numeric value without changing the gradients.

### Convexity

Convex losses are useful in simpler models, but deep neural network objectives are usually non-convex because the network itself is a nonlinear composition of parameterized layers. Neural networks can still be trained with gradient-based methods even when the total objective is non-convex.

### Exact Match to the Evaluation Metric

The training loss does not need to be the same as the final evaluation metric. It only needs to provide a useful optimization signal. Cross-entropy is commonly used even when the reported metric is accuracy, F1, BLEU, reward, or another task-specific measure.

### Zero Minimum

A loss does not need to have an achievable value of zero. Noisy labels, ambiguous targets, regularization terms, and probabilistic objectives can make zero loss impossible or meaningless.

## Practical Rule

For a loss to be usable in a standard neural network training loop, ask:

1. Does it produce one scalar to minimize?
2. Does it depend on the model outputs and parameters?
3. Can backpropagation compute useful gradients through it?
4. Is it compatible with the output layer's range and interpretation?
5. Is it numerically stable over expected training values?
6. Does lower loss correspond to better task behavior?
7. Can it be averaged over minibatches?

If the answer to any of these is no, the function may still be mathematically valid, but it is likely to fail as a neural network training objective without modification.

## Related

- [[../activations/activation-functions|activation-functions]]
- [[../activations/activation-saturation-and-gradients|activation-saturation-and-gradients]]
- [[cross-entropy-loss]]
- [[binary-cross-entropy-loss]]
- [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]]
- [[topics/statistics/wiki/information-theory/kl-divergence|kl-divergence]]
- [[../single-neurons-and-layers|single-neurons-and-layers]]
- [[topics/machine-learning/wiki/empirical-risk-minimisation|empirical-risk-minimisation]]
- [[topics/machine-learning/wiki/glossary|loss function]]

## Sources

- [PyTorch Tutorials: Automatic Differentiation with `torch.autograd`](https://docs.pytorch.org/tutorials/beginner/basics/autogradqs_tutorial.html)
- [PyTorch Tutorials: The Fundamentals of Autograd](https://docs.pytorch.org/tutorials/beginner/introyt/autogradyt_tutorial.html)
- [PyTorch Documentation: Automatic Differentiation Package](https://docs.pytorch.org/docs/2.9/autograd.html)
- [MIT 6.390 Intro to Machine Learning: Neural Networks](https://introml.mit.edu/notes/neural_networks.html)
- [Deep Learning Book: Chapter 5, Machine Learning Basics](https://www.deeplearningbook.org/contents/ml.html)
- [Deep Learning Book: Table of Contents, Chapter 6 Gradient-Based Learning](https://www.deeplearningbook.org/contents/TOC.html)
