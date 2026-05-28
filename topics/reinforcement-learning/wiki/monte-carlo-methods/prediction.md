# Monte Carlo Prediction

Monte Carlo prediction estimates the value of a fixed policy by averaging returns observed after visits to states or state-action pairs.

For a policy `pi`, the state-value target `v_pi(s)` is the expected return after starting in state `s` and then following `pi`. Monte Carlo prediction estimates this expectation from episodes generated under `pi`.

## First-Visit and Every-Visit Estimates

A visit is an occurrence of a state in an episode. A first visit is the first occurrence of that state in that episode.

Two common estimators differ only in which returns they average:

- First-visit MC averages returns after the first visit to a state in each episode.
- Every-visit MC averages returns after every visit to that state.

The same distinction applies to state-action pairs. Both methods converge to the corresponding value as the relevant number of visits grows, under the chapter's finite-variance assumptions.

## Procedure

For state-value prediction:

1. Generate an episode using the policy being evaluated.
2. For each relevant state visit, compute the return from that time step to episode termination.
3. Add the return to the state's collection of observed returns.
4. Set the value estimate to the average observed return.

This can be implemented with stored return lists or with incremental averaging.

## Why It Matters

Monte Carlo prediction is useful when sample episodes are easier to obtain than a complete model of transition and reward probabilities. Sutton and Barto use blackjack as an example: one can simulate games and average returns from states without explicitly enumerating the complicated transition probabilities.

## Limitations

- Learning waits until the return is known, so the task must be episodic or otherwise handled with episode-like truncation.
- Estimates can be sample inefficient when important states are rarely visited.
- Pure state-value prediction is not enough for model-free control unless action values are also estimated.

## Related

- [[monte-carlo-methods/overview|monte-carlo-methods]]
- [[monte-carlo-methods/action-values|monte-carlo-action-values]]
- [[monte-carlo-methods/importance-sampling|importance-sampling]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): policy evaluation by averaging returns after visits is introduced in Section 5.1; see lines 5151-5191 in the indexed PDF text.
