# TD Error

The TD error is the difference between a current value estimate and a one-step bootstrapped target.

For TD(0) state-value prediction, Sutton and Barto define the error at time `t` as:

```text
delta_t = R_{t+1} + gamma * V(S_{t+1}) - V(S_t)
```

The corresponding update is:

```text
V(S_t) <- V(S_t) + alpha * delta_t
```

## Timing

The TD error is the error in the estimate made at time `t`, but it is only available at time `t + 1`, after the next reward and next state have been observed.

## Relationship to Monte Carlo Error

If the value function does not change during an episode, the Monte Carlo error `G_t - V(S_t)` can be decomposed into a discounted sum of later TD errors.

This identity is one of the bridges between [[monte-carlo-methods/overview|monte-carlo-methods]] and [[temporal-difference-methods/overview|temporal-difference-learning]]. If values are updated during the episode, as in online TD(0), the identity only holds approximately unless correction terms are included.

## Action-Value Form

In [[temporal-difference-methods/sarsa|sarsa]], the TD error uses action values:

```text
delta_t = R_{t+1} + gamma * Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t)
```

In [[temporal-difference-methods/q-learning|q-learning]], the target replaces the next selected action with the maximum over next-state actions:

```text
delta_t = R_{t+1} + gamma * max_a Q(S_{t+1}, a) - Q(S_t, A_t)
```

## Related

- [[temporal-difference-methods/prediction|td-prediction]]
- [[temporal-difference-methods/sarsa|sarsa]]
- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/expected-sarsa|expected-sarsa]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
