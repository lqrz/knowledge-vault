# Maximization Bias

Maximization bias is the positive bias that can appear when the maximum of noisy value estimates is used as an estimate of the maximum true value.

Suppose several actions have true values near zero, but their estimates are noisy. Some estimates will be above zero and some below. Taking the maximum estimate tends to select one of the upward errors, so the maximum estimated value is biased upward relative to the maximum true value.

## In TD Control

TD control methods often use maximization to construct target policies. In [[temporal-difference-methods/q-learning|q-learning]], the update target includes:

```text
max_a Q(S_{t+1}, a)
```

When `Q` estimates are noisy, this target can overestimate the value of the next state. Sutton and Barto's maximization-bias example shows Q-learning favoring a bad action because many noisy downstream actions make the max look attractive.

## Double Learning

One response is double learning: use one value estimator to choose the maximizing action and another independent estimator to evaluate that selected action.

For Q-learning, this leads to [[temporal-difference-methods/double-q-learning|double-q-learning]], which keeps two action-value tables and updates one table using an action selected by that table but evaluated by the other table.

## Related

- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/double-q-learning|double-q-learning]]
- [[temporal-difference-methods/expected-sarsa|expected-sarsa]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
