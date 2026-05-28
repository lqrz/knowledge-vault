# Entropy

Entropy measures the average amount of information needed to communicate an outcome drawn from a probability distribution.

For a discrete distribution $p$:

$$
H(p) = \sum_x p(x)\log_2 \frac{1}{p(x)}
$$

Equivalently:

$$
H(p) = -\sum_x p(x)\log_2 p(x)
$$

The unit is bits when the logarithm is base 2.

## Coding Interpretation

Entropy is the best possible average code length for outcomes from a distribution, assuming an optimal code and enough aggregation to approach the limit.

If an event is certain, no bits are needed to identify it:

$$
H([1, 0, \ldots]) = 0
$$

If there are $N$ equally likely outcomes, entropy is:

$$
H = \log_2 N
$$

So a fair binary outcome has $1$ bit of entropy, while a uniform choice among $64$ outcomes has $6$ bits.

## Uncertainty Interpretation

Entropy is high when probability mass is spread across many plausible outcomes. It is low when probability mass is concentrated on a small number of outcomes.

This is why entropy can be read as uncertainty: the more diffuse the distribution, the more information is learned when the outcome is revealed.

## Related

- [[information-theory]]
- [[prefix-codes-and-entropy]]
- [[cross-entropy]]
- [[kl-divergence]]
- [[joint-and-conditional-entropy]]
- [[fractional-bits]]
- [[topics/deep-learning/wiki/neural-networks/losses/cross-entropy-loss|cross-entropy-loss]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
