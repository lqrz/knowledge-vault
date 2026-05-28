# TD Prediction

TD prediction estimates the value function of a fixed policy from experience using bootstrapped targets.

The simplest method is TD(0), also called one-step TD. After a transition from `S_t` to `S_{t+1}` with reward `R_{t+1}`, TD(0) updates:

```text
V(S_t) <- V(S_t) + alpha * (R_{t+1} + gamma * V(S_{t+1}) - V(S_t))
```

The target is `R_{t+1} + gamma * V(S_{t+1})`. This differs from [[monte-carlo-methods/prediction|monte-carlo-prediction]], which waits for the complete return `G_t`.

## Bootstrapping

TD(0) is a bootstrapping method because the update target includes the current estimate `V(S_{t+1})`. It is also a sample update because it uses a single observed transition rather than a full expectation over all possible successor states.

This places TD prediction between Monte Carlo prediction and dynamic-programming policy evaluation:

- Monte Carlo uses sampled complete returns and does not bootstrap.
- Dynamic programming bootstraps from successor values but requires a model.
- TD samples one transition and bootstraps from the next state's current value estimate.

## Advantages

TD prediction can be implemented online and incrementally. It only needs to wait one time step before updating, while Monte Carlo prediction must wait until the relevant return is known.

Sutton and Barto highlight several consequences:

- TD can learn during very long episodes.
- TD applies naturally to continuing tasks.
- TD can update from each transition even when later exploratory actions would make a full Monte Carlo return unsuitable for evaluating a target policy.
- In practice, TD(0) often learns faster than constant-step-size Monte Carlo on stochastic prediction tasks, though a general proof that TD always converges faster is not available.

## Batch TD

Under batch updating, TD(0) repeatedly processes a fixed data set until the value function converges. Sutton and Barto contrast the batch TD answer with the batch Monte Carlo answer:

- Batch Monte Carlo minimizes squared error on the observed returns in the training set.
- Batch TD(0) converges to the certainty-equivalence estimate: the value function that would be exactly correct for the maximum-likelihood Markov reward process implied by the observed transitions and rewards.

This helps explain why TD can generalize better from limited Markov data than simply averaging observed returns after each state.

## Related

- [[temporal-difference-methods/overview|temporal-difference-learning]]
- [[temporal-difference-methods/td-error|td-error]]
- [[monte-carlo-methods/prediction|monte-carlo-prediction]]
- [[prediction-and-control]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
