# Double Q-Learning

Double Q-learning is a TD control algorithm designed to reduce [[temporal-difference-methods/maximization-bias|maximization-bias]] in Q-learning.

It keeps two action-value estimates, commonly written `Q1` and `Q2`. On each step, it updates one of them and uses the other to evaluate the greedy action chosen by the updated table.

For example, when updating `Q1`:

```text
Q1(S_t, A_t) <- Q1(S_t, A_t)
  + alpha * (R_{t+1}
    + gamma * Q2(S_{t+1}, argmax_a Q1(S_{t+1}, a))
    - Q1(S_t, A_t))
```

The symmetric update for `Q2` chooses the maximizing action according to `Q2` and evaluates it with `Q1`.

## Why It Helps

The bias in [[temporal-difference-methods/q-learning|q-learning]] comes partly from using the same noisy estimates both to choose the maximizing action and to estimate that action's value. Double Q-learning separates these roles.

The behavior policy can still use both estimates, for example by acting epsilon-greedily with respect to `Q1 + Q2`.

## Cost

Double Q-learning roughly doubles memory because it stores two value tables. Sutton and Barto note that the per-step computational cost need not double because only one table is updated on each step.

## Related

- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/maximization-bias|maximization-bias]]
- [[temporal-difference-methods/expected-sarsa|expected-sarsa]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
