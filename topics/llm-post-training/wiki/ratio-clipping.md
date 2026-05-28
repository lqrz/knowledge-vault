# Ratio Clipping

Ratio clipping limits how much any single token probability ratio can influence an RL fine-tuning update. It is used in GRPO to reduce unstable training caused by very large or very small policy-to-reference probability ratios.

The GRPO transcript describes clipping each token's ratio between `1 - epsilon` and `1 + epsilon`, with `epsilon = 0.2` given as a practical example. With that setting, ratios are clipped into the range `[0.8, 1.2]` before they are multiplied by the [[advantages|advantage]].[^source]

## What Gets Clipped

The clipping is applied to each token's probability ratio, not to the final aggregate loss. This distinction matters because a few extreme token ratios can otherwise dominate the [[grpo-loss|GRPO loss]]. The transcript gives an example ratio around `1370`, which would produce a very large negative loss when paired with a high positive advantage.[^source]

## Clipped Objective

The transcript describes computing both an unclipped and clipped version of the token-level objective:

```text
unclipped = ratio * advantage
clipped = clamp(ratio, 1 - epsilon, 1 + epsilon) * advantage
policy_loss = min(unclipped, clipped)
```

The purpose is to avoid overly large single-step updates while still allowing the policy model to learn from reward-weighted completions.[^source]

## Related Pages

- [[grpo-loss]]
- [[advantages]]
- [[policy-and-reference-models]]
- [[kl-divergence-penalty]]

## Sources

[^source]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
