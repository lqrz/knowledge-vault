# Prediction and Control

In reinforcement learning, **prediction** and **control** name two different problem types.

**Prediction** means estimating the return produced by a fixed policy. The policy is treated as given, and the learning problem is to estimate a value function such as `v_pi(s)` or `q_pi(s, a)`. In [[monte-carlo-methods/prediction|monte-carlo-prediction]], this is done by averaging observed returns after visits to states or state-action pairs.

**Control** means finding or improving a policy so that expected return increases. Control usually includes prediction as a subproblem, because the agent needs value estimates to decide which actions or policies are better. In [[monte-carlo-methods/control|monte-carlo-control]], sampled returns estimate action values, and policy improvement changes `pi` toward actions with higher estimated value.

## Practical Difference

Prediction asks: given policy `pi`, how good is it?

Control asks: what policy should the agent use?

This distinction explains why [[monte-carlo-methods/action-values|monte-carlo-action-values]] matter for model-free control. A state-value estimate can say how good a state is under a policy, but without a model it does not directly reveal which action caused better future returns. Action-value estimates let the agent compare available actions directly.

## Related

- [[glossary]]
- [[monte-carlo-methods/overview|monte-carlo-methods]]
- [[monte-carlo-methods/prediction|monte-carlo-prediction]]
- [[monte-carlo-methods/control|monte-carlo-control]]
- [[monte-carlo-methods/action-values|monte-carlo-action-values]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [[monte-carlo-methods/overview|monte-carlo-methods]]: summarizes the prediction/control split in Monte Carlo methods.
- [[monte-carlo-methods/prediction|monte-carlo-prediction]]: defines prediction as estimating values for a fixed policy from sampled returns.
- [[monte-carlo-methods/control|monte-carlo-control]]: defines control as combining value estimation with policy improvement.
- [[monte-carlo-methods/action-values|monte-carlo-action-values]]: explains why action values are needed for model-free control.
