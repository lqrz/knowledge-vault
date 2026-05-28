# Monte Carlo Action Values

Monte Carlo action-value estimation learns `q_pi(s, a)`, the expected return after starting in state `s`, taking action `a`, and then following policy `pi`.

Action values are central in model-free reinforcement learning because, without a model, state values alone do not tell the agent which action leads to better future returns.

## State Values Versus Action Values

With a model, an agent can use a state-value function plus transition dynamics to look one step ahead and choose an action. Without a model, that one-step lookahead is unavailable. The agent therefore needs direct estimates for each action available in each state.

Monte Carlo action-value prediction mirrors state-value prediction:

- A state-action pair is visited when the episode enters state `s` and action `a` is selected.
- First-visit MC averages returns after the first visit to the pair in an episode.
- Every-visit MC averages returns after all visits to the pair.

## Exploration Requirement

The main complication is coverage. If the policy is deterministic, following it may produce returns for only one action from each state. The other actions receive no samples, so their estimates cannot improve.

This creates the exploration problem for Monte Carlo control:

- To evaluate alternatives, returns must be observed for all relevant actions.
- [[monte-carlo-methods/exploring-starts|exploring-starts]] is one idealized way to force coverage.
- [[monte-carlo-methods/on-policy-control|on-policy-monte-carlo-control]] keeps the policy soft.
- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]] learns about a target policy from a separate exploratory behavior policy.

## Related

- [[monte-carlo-methods/prediction|monte-carlo-prediction]]
- [[monte-carlo-methods/control|monte-carlo-control]]
- [[monte-carlo-methods/exploring-starts|exploring-starts]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): Section 5.2 motivates action values for model-free control and explains the coverage problem; see lines 5322-5350 in the indexed PDF text.
