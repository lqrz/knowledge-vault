# Probability Distributions

A probability distribution assigns probability mass or density to possible outcomes. For a discrete variable $X$, the distribution is the set of values $p(x)$ for each possible outcome $x$.

For two discrete variables $X$ and $Y$, the joint distribution $p(x, y)$ assigns probability to outcome pairs.

## Marginal Probability

A marginal probability describes one variable without conditioning on another variable.

For example:

$$
p(x)
$$

$$
p(y)
$$

If the full joint distribution is known, a marginal can be recovered by summing out the other variable:

$$
p(x) = \sum_y p(x, y)
$$

$$
p(y) = \sum_x p(x, y)
$$

## Independence

Two variables are independent when knowing one does not change the probability of the other:

$$
p(x, y) = p(x)p(y)
$$

Equivalently:

$$
p(y \mid x) = p(y)
$$

$$
p(x \mid y) = p(x)
$$

In Olah's visual explanation, independence appears as a rectangular grid whose row and column boundaries pass through cleanly. Dependence appears when some joint cells gain probability mass and others lose it.

## Related

- [[conditional-probability]]
- [[../information-theory/joint-and-conditional-entropy|joint-and-conditional-entropy]]
- [[../information-theory/mutual-information|mutual-information]]
- [[../statistical-paradoxes/simpsons-paradox|simpsons-paradox]]

## Sources

- [[../../raw/articles/colah/visual-information-theory|Visual Information Theory]] by Christopher Olah, 2015.
