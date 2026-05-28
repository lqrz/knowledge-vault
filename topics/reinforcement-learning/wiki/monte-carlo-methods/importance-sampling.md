# Importance Sampling

Importance sampling is a correction technique for estimating expectations under one distribution using samples from another distribution.

In off-policy Monte Carlo reinforcement learning, it corrects returns sampled under a behavior policy so they estimate values for a target policy.

## Role in Off-Policy Monte Carlo

For a sampled trajectory, the importance-sampling ratio compares the probability of the observed actions under the target policy with their probability under the behavior policy. Returns are weighted by these ratios before being averaged.

Coverage is required: the behavior policy must assign nonzero probability to every action that the target policy might take. Otherwise a required ratio may be undefined or impossible to estimate from behavior-policy data.

## Ordinary Versus Weighted Importance Sampling

Sutton and Barto distinguish two estimators:

- Ordinary importance sampling uses a simple average of ratio-weighted returns.
- Weighted importance sampling normalizes by the sum of weights.

Ordinary importance sampling can be unbiased but may have very large or even infinite variance. Weighted importance sampling is biased in finite samples but is usually preferred in practice because its variance is much lower and finite in the cases discussed in the chapter.

## Incremental Implementation

Weighted importance sampling can be maintained incrementally by tracking both the current value estimate and the cumulative weight. Each new weighted return adjusts the estimate in proportion to its weight relative to the accumulated total.

## Return-Specific Refinements

Sutton and Barto also describe return-specific importance-sampling ideas, including discounting-aware and per-reward variants. These exploit the internal structure of discounted returns instead of applying one full-trajectory ratio blindly to the entire return.

## Related

- [[monte-carlo-methods/off-policy-control|off-policy-monte-carlo]]
- [[monte-carlo-methods/prediction|monte-carlo-prediction]]
- [[monte-carlo-methods/control|monte-carlo-control]]

## Sources

- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5 online copy](https://web.stanford.edu/class/psych209/Readings/SuttonBartoRL.pdf): Sections 5.5, 5.6, and 5.8 discuss ordinary, weighted, incremental, and return-specific importance sampling; see lines 6183-6223, 6431-6463, and 6632-6637 in the indexed PDF text.
