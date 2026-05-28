# KL Divergence

Kullback-Leibler divergence measures how much extra information is needed when outcomes from one distribution are represented using a distribution that does not match them.

For true distribution $p$ and reference or model distribution $q$:

$$
D_{\mathrm{KL}}(p \parallel q) = \sum_x p(x)\log_2 \frac{p(x)}{q(x)}
$$

Equivalently:

$$
D_{\mathrm{KL}}(p \parallel q) = H(p, q) - H(p)
$$

where $H(p, q)$ is [[cross-entropy]] and $H(p)$ is [[entropy]].

## Interpretation

KL divergence is the expected extra code length caused by using a code optimized for $q$ when the data actually comes from $p$.

If $p = q$, no extra bits are needed:

$$
D_{\mathrm{KL}}(p \parallel q) = 0
$$

As the distributions diverge, the expected extra length increases.

## Not a Metric

KL divergence is often described as distance-like, but it is not a true metric:

- it is not symmetric
- it does not generally satisfy the triangle inequality

The direction matters:

$$
D_{\mathrm{KL}}(p \parallel q) \neq D_{\mathrm{KL}}(q \parallel p)
$$

## Machine Learning Connection

KL divergence is useful when a model distribution should remain close to another distribution. In LLM post-training, a [[topics/llm-post-training/wiki/kl-divergence-penalty|KL divergence penalty]] can discourage a policy model from drifting too far from a reference model.

In classification, minimizing [[cross-entropy]] against a fixed empirical target also minimizes KL divergence up to the target distribution's constant entropy term.

## Related

- [[cross-entropy]]
- [[entropy]]
- [[mutual-information]]
- [[information-theory]]
- [[topics/llm-post-training/wiki/kl-divergence-penalty|kl-divergence-penalty]]
- [[topics/deep-learning/wiki/neural-networks/losses/cross-entropy-loss|cross-entropy-loss]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
