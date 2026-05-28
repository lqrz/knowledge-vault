# Monte Carlo Control

Monte Carlo control uses sampled returns to estimate action values and improve a policy. It adapts general policy iteration to the setting where value functions are learned from episodes rather than computed from a known model.

## General Policy Iteration

The control loop maintains:

- An action-value estimate `Q`.
- A policy `pi`.

Monte Carlo evaluation improves `Q` using returns from episodes. Policy improvement then changes `pi` to favor actions with higher estimated action value.

In the idealized policy-iteration view, evaluation could be run to completion before each policy improvement step. In practical Monte Carlo control, evaluation and improvement are usually interleaved episode by episode.

## Why Action Values Are Used

Monte Carlo control usually estimates `q_pi` or `q_*`, not only `v_pi`, because model-free action selection requires comparing actions directly. A state-value estimate says how good a state is under a policy; it does not by itself identify which action should be taken when transition dynamics are unknown.

## Exploration Approaches

Sutton and Barto present several ways to maintain exploration:

- [[monte-carlo-methods/exploring-starts|exploring-starts]]: assume episodes can start from all state-action pairs.
- [[monte-carlo-methods/on-policy-control|on-policy-monte-carlo-control]]: use an epsilon-soft or epsilon-greedy policy that continues to sample all actions.
- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]]: use a behavior policy for exploration while learning a distinct target policy.

## Caveats

Monte Carlo control inherits the strengths and weaknesses of Monte Carlo estimation:

- It can learn directly from sampled episodes.
- It does not need a transition model.
- It waits for episode returns.
- It can be high variance, especially in off-policy settings using importance sampling.
- Its convergence assumptions can be idealized, especially for exploring starts and exact policy evaluation.

## Related

- [[monte-carlo-methods/overview|monte-carlo-methods]]
- [[monte-carlo-methods/action-values|monte-carlo-action-values]]
- [[monte-carlo-methods/on-policy-control|on-policy-monte-carlo-control]]
- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): Section 5.3 presents Monte Carlo control as general policy iteration using sampled returns; see lines 5362-5378 in the indexed PDF text.
