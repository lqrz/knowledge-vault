# Expected Sarsa

Expected Sarsa is a temporal-difference control method that updates an action value toward the expected next action value under a policy.

Its tabular update is:

```text
Q(S_t, A_t) <- Q(S_t, A_t)
  + alpha * (R_{t+1}
    + gamma * sum_a pi(a | S_{t+1}) * Q(S_{t+1}, a)
    - Q(S_t, A_t))
```

This replaces the sampled next action in [[temporal-difference-methods/sarsa|sarsa]] with an expectation over possible next actions.

## Variance

Expected Sarsa removes variance due to randomly selecting `A_{t+1}` under the policy, at the cost of computing the expectation over actions. When the action set is small or the policy probabilities are easy to compute, this extra cost can be modest.

Sutton and Barto report that Expected Sarsa can outperform Sarsa across a broad range of step sizes in the cliff-walking task.

## Relationship to Q-Learning

Expected Sarsa can be used on-policy or off-policy. If the target policy is greedy, then the expected next action value reduces to the maximum next action value, making Expected Sarsa equivalent to [[temporal-difference-methods/q-learning|q-learning]] for that target.

This is why Sutton and Barto describe Expected Sarsa as a generalization that can subsume Q-learning while often improving over sampled Sarsa.

## Related

- [[temporal-difference-methods/sarsa|sarsa]]
- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/td-error|td-error]]
- [[temporal-difference-methods/overview|temporal-difference-learning]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
