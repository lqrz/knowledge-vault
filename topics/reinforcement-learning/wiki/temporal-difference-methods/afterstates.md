# Afterstates

An afterstate is the state-like situation immediately after the agent's action has had its known direct effect, but before the environment's remaining response is resolved.

Afterstate value functions can be useful when the agent knows the immediate consequences of its own actions. Instead of learning values for every state-action pair separately, the agent can learn values for the resulting afterstates.

## Games

Sutton and Barto use games such as tic-tac-toe as a motivating example. The agent knows exactly how its move changes the board, but not how the opponent will reply.

Different position-move pairs can produce the same resulting board position. A conventional action-value function would learn separate values for those pairs, while an afterstate value function shares the value through the common resulting position.

## Other Tasks

Afterstates can also appear outside games. In queuing problems, actions such as assigning a customer to a server or rejecting a customer may have immediate known effects before the next random arrival or service event.

The key requirement is partial known dynamics: the action's immediate effect is known well enough that the post-action situation is a useful value-function argument.

## Relationship to GPI

Afterstate methods are still compatible with generalized policy iteration. The policy and afterstate value function interact like ordinary policy and value estimates, but the value function is defined over post-action situations rather than ordinary decision states or state-action pairs.

## Related

- [[temporal-difference-methods/overview|temporal-difference-learning]]
- [[prediction-and-control]]
- [[temporal-difference-methods/q-learning|q-learning]]
- [[temporal-difference-methods/sarsa|sarsa]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
