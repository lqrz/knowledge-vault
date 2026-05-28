# Sarsa

Sarsa is an on-policy temporal-difference control algorithm for estimating action values.

It updates `Q(S_t, A_t)` from the next reward and the action value of the next state-action pair actually selected by the current behavior policy:

```text
Q(S_t, A_t) <- Q(S_t, A_t)
  + alpha * (R_{t+1} + gamma * Q(S_{t+1}, A_{t+1}) - Q(S_t, A_t))
```

The name comes from the transition tuple used in the update: state, action, reward, next state, next action.

## On-Policy Control

Sarsa continually estimates the action-value function for the same policy it uses to behave. In a common tabular setup, the agent behaves epsilon-greedily with respect to `Q` and improves as `Q` changes.

Because the update target includes `A_{t+1}` sampled from the behavior policy, Sarsa accounts for the consequences of ongoing exploration. This distinguishes it from [[temporal-difference-methods/q-learning|q-learning]], whose target assumes greedy action selection at the next state.

## Exploration

Sutton and Barto state that Sarsa can converge to an optimal policy and action-value function when all state-action pairs are visited infinitely often and the policy converges in the limit to a greedy policy.

With persistent epsilon-greedy exploration, Sarsa may learn safer online behavior than Q-learning in tasks where exploratory actions are risky. The cliff-walking example illustrates this: Sarsa learns a safer path because it evaluates the exploratory policy, while Q-learning learns values for the greedy optimal path and can perform worse online while exploration continues.

## Related

- [[temporal-difference-methods/overview|temporal-difference-learning]]
- [[temporal-difference-methods/td-error|td-error]]
- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/expected-sarsa|expected-sarsa]]
- [[monte-carlo-methods/on-policy-control|on-policy-monte-carlo-control]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
