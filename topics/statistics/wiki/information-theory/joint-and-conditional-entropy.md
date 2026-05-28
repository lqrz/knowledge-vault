# Joint and Conditional Entropy

Joint entropy measures uncertainty over multiple variables together. Conditional entropy measures the uncertainty that remains in one variable after another variable is known.

## Joint Entropy

For two discrete variables $X$ and $Y$:

$$
H(X, Y) = \sum_{x,y} p(x, y)\log_2 \frac{1}{p(x, y)}
$$

This is the entropy of the joint distribution over outcome pairs.

## Conditional Entropy

Conditional entropy measures how many bits are needed, on average, to communicate $X$ to someone who already knows $Y$:

$$
H(X \mid Y) = \sum_y p(y)\sum_x p(x \mid y)\log_2 \frac{1}{p(x \mid y)}
$$

Equivalently:

$$
H(X \mid Y) = \sum_{x,y} p(x, y)\log_2 \frac{1}{p(x \mid y)}
$$

If knowing $Y$ strongly predicts $X$, then $H(X \mid Y)$ is small.

## Probability Chain Rule

The entropy decomposition depends on the probability chain rule:

$$
p(x, y) = p(y)p(x \mid y)
$$

Equivalently:

$$
p(x, y) = p(x)p(y \mid x)
$$

This says a joint probability can be factored into the probability of one variable and the conditional probability of the other variable given the first.

## Entropy Chain Rule

Joint entropy decomposes into known information plus remaining uncertainty:

$$
H(X, Y) = H(Y) + H(X \mid Y)
$$

Also:

$$
H(X, Y) = H(X) + H(Y \mid X)
$$

The entropy chain rule follows from the probability chain rule because:

$$
\log_2 \frac{1}{p(x, y)}
= \log_2 \frac{1}{p(y)p(x \mid y)}
= \log_2 \frac{1}{p(y)} + \log_2 \frac{1}{p(x \mid y)}
$$

Taking the expectation over the joint distribution separates the joint uncertainty into the uncertainty of $Y$ plus the remaining uncertainty of $X$ after $Y$ is known.

The reason the entropies add is not that the variables are independent. They add because the information content of a joint event factors into two sequential pieces:

1. the information needed to identify $Y$
2. the extra information needed to identify $X$ once $Y$ is known

For one outcome pair $(x, y)$:

$$
\text{information in }(x,y)
= \text{information in }y + \text{information in }x\text{ given }y
$$

In symbols:

$$
-\log_2 p(x,y)
= -\log_2 p(y) - \log_2 p(x \mid y)
$$

Averaging this identity over all $(x,y)$ gives:

$$
\mathbb{E}_{x,y}[-\log_2 p(x,y)]
= \mathbb{E}_{y}[-\log_2 p(y)]
+ \mathbb{E}_{x,y}[-\log_2 p(x \mid y)]
$$

which is:

$$
H(X,Y) = H(Y) + H(X \mid Y)
$$

So the sum means "first encode $Y$, then encode whatever uncertainty about $X$ remains after $Y$ is known."

## Inequalities

For ordinary discrete entropy:

$$
H(X, Y) \geq H(X) \geq H(X \mid Y)
$$

The first inequality says communicating two variables requires at least as much information as communicating one of them. The second says that knowing $Y$ cannot increase the remaining uncertainty about $X$ on average.

## Related

- [[entropy]]
- [[mutual-information]]
- [[../probability/conditional-probability|conditional-probability]]
- [[../probability/probability-distributions|probability-distributions]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
