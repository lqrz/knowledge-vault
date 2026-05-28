# KL Divergence Penalty

A KL divergence penalty discourages a trainable policy model from drifting too far from a frozen reference model during reinforcement-style fine-tuning.

For the underlying information-theory quantity, see [[topics/statistics/wiki/information-theory/kl-divergence|kl-divergence]].

In the GRPO transcript, KL divergence is the final component added to the loss after the policy ratio, advantage weighting, and [[ratio-clipping|ratio clipping]]. It is described as a per-token penalty based on the difference between policy-model and reference-model log probabilities.[^source]

## Intuition

The transcript defines a delta-like quantity as the difference between policy and reference log probabilities. If the policy model is more confident than the reference model on a generated token, the difference is positive. If it is less confident, the difference is negative.[^source]

The KL penalty acts as a control term: it allows the model to learn task-specific behavior, but discourages large deviations from the reference model's distribution. This is especially important in RL-style training, where optimizing reward too aggressively can make the model unstable or overly specialized.

## Beta

The penalty is weighted by a `beta` parameter. A beta of zero removes the KL term. Larger beta values make the model stay closer to the reference model.[^source]

The transcript gives these practical claims:

- `beta = 0` skips the KL penalty.
- `beta = 0.1` is presented as a typical starting value from Predibase.
- `beta = 0.2` to `0.5` may be useful when preserving generality matters more.[^source]

These beta values are course guidance from the raw transcript, not a universal rule. They should be treated as task- and implementation-dependent.

## Related Pages

- [[grpo-loss]]
- [[policy-and-reference-models]]
- [[ratio-clipping]]
- [[fine-tuning-map]]
- [[topics/statistics/wiki/information-theory/kl-divergence|kl-divergence]]
- [[topics/statistics/wiki/information-theory/cross-entropy|cross-entropy]]

## Sources

[^source]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
