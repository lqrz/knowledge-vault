# Empirical Risk Minimisation

Empirical risk minimisation is the learning principle of choosing a model that minimises the average loss measured on a finite training dataset.

The key idea is that the true risk of a model is usually defined over an unknown data-generating distribution, but training only gives access to sampled examples. Empirical risk replaces the unreachable population expectation with an average over observed data.

## Definition

Given a model $f_\theta$, a loss function $\ell$, and a training dataset:

$$
\mathcal{D}_{train} = \{(x_i, y_i)\}_{i=1}^{n}
$$

the empirical risk is:

$$
\hat{R}(\theta) = \frac{1}{n}\sum_{i=1}^{n}\ell(f_\theta(x_i), y_i)
$$

Empirical risk minimisation chooses parameters:

$$
\hat{\theta} = \arg\min_{\theta}\hat{R}(\theta)
$$

In words: find the model parameters that make the average training loss as small as possible.

## Relationship to True Risk

The true risk is the expected loss on the underlying data distribution:

$$
R(\theta) = \mathbb{E}_{(x,y)\sim P}[\ell(f_\theta(x), y)]
$$

The true distribution $P$ is usually unknown, so it cannot be minimised directly. Empirical risk minimisation uses the training set as a finite approximation to that distribution.

This creates a central machine learning tension: lowering empirical risk is useful only if it also lowers true risk. When a model minimises training loss by fitting sample-specific noise, it may [[glossary|overfit]] and generalise poorly.

## Why It Matters

Empirical risk minimisation connects several core machine learning concepts:

- [[glossary|Datasets]] provide the finite sample used to estimate risk.
- [[glossary|Loss functions]] define what counts as error.
- [[glossary|Optimization]] searches for parameters with lower empirical risk.
- [[glossary|Generalization]] determines whether lower training loss translates into better performance on new data.
- Regularisation, validation, and model selection are ways to keep empirical risk minimisation from selecting models that only memorise the training set.

## Examples

In linear regression, empirical risk minimisation often means choosing coefficients that minimise mean squared error on the training data.

In classification, it may mean minimising cross-entropy loss or another surrogate loss because directly minimising classification error can be difficult to optimise.

In neural network training, gradient-based optimisation usually searches for parameters with low empirical risk, often with additional regularisation or early stopping.

## Limits

Empirical risk minimisation depends on the training data being informative about the deployment distribution. It can fail when:

- the training set is too small;
- the training examples are biased or unrepresentative;
- the model class is flexible enough to memorise noise;
- the loss function is misaligned with the real goal;
- the deployment distribution differs from the training distribution.

## Related

- [[glossary|loss function]]
- [[glossary|optimization]]
- [[glossary|generalization]]
- [[topics/deep-learning/wiki/index|deep-learning]]

## Sources

- [[AGENTS|LLM Research Vault Instructions]]
- User-provided seed concept: "empirical risk minimisation". Add textbook or course citations when this page is expanded.
