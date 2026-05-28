# Temporal-Difference Methods

Temporal-difference methods are a reinforcement-learning method family that learns from sampled experience while bootstrapping from current value estimates.

Sutton and Barto frame TD learning as a combination of [[monte-carlo-methods/overview|Monte Carlo methods]] and dynamic programming:

- Like Monte Carlo methods, TD methods can learn directly from experience without a model of the environment dynamics.
- Like dynamic programming, TD methods update a value estimate using another learned value estimate rather than waiting for a complete return.

This makes TD methods central to online, model-free reinforcement learning.

## Prediction

In the prediction problem, TD methods estimate the value function of a fixed policy. The simplest case is [[temporal-difference-methods/prediction|TD(0)]], which updates a state value after one transition using the observed reward plus the current value estimate of the next state.

The update target is a one-step bootstrapped target rather than a full episode return. This lets learning happen before an episode terminates and also works naturally in continuing tasks that do not have terminal episodes.

## Control

TD control combines TD prediction with [[prediction-and-control|policy improvement]] through generalized policy iteration.

Chapter 6 presents three one-step, tabular, model-free TD control methods:

- [[temporal-difference-methods/sarsa|sarsa]]: on-policy TD control that evaluates the same exploratory policy used for behavior.
- [[temporal-difference-methods/q-learning|q-learning]]: off-policy TD control that learns greedy optimal action values independent of the behavior policy, assuming sufficient state-action coverage.
- [[temporal-difference-methods/expected-sarsa|expected-sarsa]]: a TD control update that uses the expected value of next actions under a policy, reducing variance relative to Sarsa and generalizing Q-learning in some settings.

## Why It Matters

TD methods are useful because they are incremental, online, and computationally small. They can update from every transition, which is important for long episodes, continuing tasks, and settings where Monte Carlo returns are delayed or unavailable.

The chapter also emphasizes that the one-step tabular methods are only the starting point. Later extensions include n-step TD, model-based planning variants, eligibility traces such as TD(lambda), and function approximation.

## Related

- [[temporal-difference-methods/prediction|td-prediction]]
- [[temporal-difference-methods/td-error|td-error]]
- [[temporal-difference-methods/sarsa|sarsa]]
- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/expected-sarsa|expected-sarsa]]
- [[temporal-difference-methods/maximization-bias|maximization-bias]]
- [[temporal-difference-methods/afterstates|afterstates]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
