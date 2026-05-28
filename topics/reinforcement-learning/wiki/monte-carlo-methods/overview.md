# Monte Carlo Methods

Monte Carlo methods estimate reinforcement-learning quantities by averaging returns from sampled episodes. They learn from experience rather than from a full model of the environment dynamics, and they do not bootstrap from other learned value estimates.

In Sutton and Barto's chapter, Monte Carlo methods are introduced as the first sample-based route from dynamic-programming ideas to model-free reinforcement learning. The same general policy iteration pattern still applies: evaluate a policy from returns, then improve the policy with respect to the estimated action values.

## Core Idea

A return sampled after a visit to a state or state-action pair is treated as one sample of that state's or pair's long-run value. Repeated samples are averaged to estimate the expected return.

This makes Monte Carlo methods natural for episodic tasks:

- The full return is known only after the episode reaches termination.
- Updates can use real experience or simulated episodes.
- A transition model `p(s', r | s, a)` is not required.
- Estimates for different states do not depend on each other through bootstrapping.

## Prediction and Control

Monte Carlo methods split into the same broad problems used elsewhere in reinforcement learning:

- [[monte-carlo-methods/prediction|monte-carlo-prediction]] estimates `v_pi` or `q_pi` for a fixed policy.
- [[monte-carlo-methods/action-values|monte-carlo-action-values]] are especially important when no model is available, because state values alone do not say which action to select.
- [[monte-carlo-methods/control|monte-carlo-control]] combines value estimation with policy improvement through general policy iteration.

## Exploration Problem

Monte Carlo control needs returns for enough state-action pairs to compare actions. Deterministic policies may never try nonselected actions, so their action values never improve.

Sutton and Barto discuss two main responses:

- [[monte-carlo-methods/exploring-starts|exploring-starts]] assumes episodes can begin from every state-action pair with nonzero probability.
- [[monte-carlo-methods/on-policy-control|on-policy-monte-carlo-control]] keeps the behavior policy soft, commonly with epsilon-greedy action selection.
- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]] separates the exploratory behavior policy from the target policy being evaluated or improved.

## Relationship to Dynamic Programming and TD Learning

Monte Carlo methods differ from dynamic programming in two central ways: they use sampled experience rather than a model, and their updates wait for complete returns rather than bootstrapping from estimated successor values.

This is why state estimates are independent in the Monte Carlo update. For a visited state `s`, the target is the realized return after that visit, such as `G_t = R_{t+1} + gamma R_{t+2} + ...` through the end of the episode. Updating `V(s)` only requires the collection or running average of returns observed after visits to `s`. It does not require the current estimate of `V(s')` for successor states.

Dynamic programming updates are different because they use Bellman backups. A policy-evaluation backup for a state depends on the model's transition and reward probabilities and on the current estimates of successor states, e.g. expected immediate reward plus discounted `V(s')`. When `V(s')` changes, the backup target for predecessor states changes too. The value estimates are therefore coupled through the transition graph.

This also sets up the motivation for temporal-difference methods: TD methods learn from sampled experience like Monte Carlo methods, but bootstrap like dynamic programming.

See [[temporal-difference-methods/overview|Temporal-difference methods]] for the follow-on method family that combines sample-based learning with bootstrapped targets.

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): Monte Carlo methods average returns from sampled episodes and do not bootstrap; see Chapter 5, especially lines 5151-5191 and 6638-6644 in the indexed PDF text.
- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
