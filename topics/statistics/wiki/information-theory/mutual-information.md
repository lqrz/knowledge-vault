# Mutual Information

Mutual information measures how much information two variables share.

One definition is:

$$
I(X, Y) = H(X) + H(Y) - H(X, Y)
$$

This says mutual information is the overlap between the information in $X$ and the information in $Y$.

## Conditional Entropy Form

Mutual information can also be written as the reduction in uncertainty about one variable after observing another:

$$
I(X, Y) = H(X) - H(X \mid Y)
$$

$$
I(X, Y) = H(Y) - H(Y \mid X)
$$

If knowing $Y$ tells you a lot about $X$, then $H(X \mid Y)$ is much smaller than $H(X)$, so mutual information is large.

## KL Divergence Form

Expanded over a joint distribution:

$$
I(X, Y) = \sum_{x,y} p(x, y)\log_2 \frac{p(x, y)}{p(x)p(y)}
$$

This is the [[kl-divergence]] between the true joint distribution $p(x, y)$ and the independent approximation $p(x)p(y)$.

So mutual information measures how many bits are saved by modeling the relationship between variables instead of assuming independence.

## Variation of Information

Variation of information measures the information not shared between two variables:

$$
V(X, Y) = H(X, Y) - I(X, Y)
$$

Unlike KL divergence, variation of information is a metric between jointly distributed variables. KL divergence compares two distributions over the same variable or variables.

## Related

- [[joint-and-conditional-entropy]]
- [[kl-divergence]]
- [[entropy]]
- [[../probability/probability-distributions|probability-distributions]]
- [[../probability/conditional-probability|conditional-probability]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
