# Conditional Probability

Conditional probability describes the probability of one event when another event is known.

$$
p(y \mid x)
$$

This reads as "the probability of $y$ given $x$."

## Product Rule

The joint probability of two variables can be factored into a marginal probability and a conditional probability:

$$
p(x, y) = p(x)p(y \mid x)
$$

The same joint distribution can also be factored in the other direction:

$$
p(x, y) = p(y)p(x \mid y)
$$

The choice of factorization does not have to match causality. Weather may causally affect clothing, but the joint distribution can still be written as either $p(\text{weather})p(\text{clothing} \mid \text{weather})$ or $p(\text{clothing})p(\text{weather} \mid \text{clothing})$.

## Bayes Connection

Because both factorizations describe the same joint probability:

$$
p(x)p(y \mid x) = p(y)p(x \mid y)
$$

Rearranging gives Bayes' theorem:

$$
p(x \mid y) = \frac{p(x)p(y \mid x)}{p(y)}
$$

## Why It Matters

Conditioning can reveal structure hidden by aggregate probabilities. This is central to [[../statistical-paradoxes/simpsons-paradox|Simpson's paradox]], where an aggregate trend can reverse after conditioning on a confounding variable.

Conditional distributions also underlie [[../information-theory/joint-and-conditional-entropy|conditional entropy]], which measures how much uncertainty about one variable remains after another variable is known.

## Related

- [[probability-distributions]]
- [[../statistical-paradoxes/simpsons-paradox|simpsons-paradox]]
- [[../information-theory/joint-and-conditional-entropy|joint-and-conditional-entropy]]
- [[../information-theory/mutual-information|mutual-information]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
