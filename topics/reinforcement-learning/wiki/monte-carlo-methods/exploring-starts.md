# Exploring Starts

Exploring starts is an assumption used in some Monte Carlo methods: every state-action pair has nonzero probability of being selected as the start of an episode.

The assumption ensures that, in the limit of infinitely many episodes, all state-action pairs are visited. This gives Monte Carlo action-value estimates enough coverage to compare actions.

## Why It Is Useful

Monte Carlo control needs returns for alternative actions. If a deterministic policy is followed from ordinary start states, many state-action pairs may never appear in the data. Their values then remain arbitrary.

Exploring starts solves this coverage issue by controlling the initial state and initial action.

## Why It Is Unrealistic

Exploring starts is often unavailable in real interaction. An agent may not be able to reset the environment to arbitrary states, and it may not be able to choose arbitrary starting actions. Sutton and Barto present it as a useful simplifying assumption, then move to methods that avoid it.

## Alternatives

- [[monte-carlo-methods/on-policy-control|on-policy-monte-carlo-control]] avoids exploring starts by keeping the policy soft.
- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]] uses a soft behavior policy to explore while learning a separate target policy.

## Related

- [[monte-carlo-methods/action-values|monte-carlo-action-values]]
- [[monte-carlo-methods/control|monte-carlo-control]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): Section 5.2 defines exploring starts and notes why the assumption is not generally reliable in direct environment interaction; see lines 5340-5350 in the indexed PDF text.
