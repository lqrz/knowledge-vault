# Advantages

An advantage is a normalized or relative reward signal that says whether a sampled completion performed better or worse than a baseline. In GRPO-style training, advantages weight the loss so that better-than-baseline completions are reinforced and worse-than-baseline completions are discouraged.

The GRPO loss transcript treats the advantage as an input computed before the loss function, often from programmatic reward functions. The policy ratio is multiplied by this advantage, so positive advantages push the policy toward the generated tokens and negative advantages push it away.[^source]

## Why Advantage Range Matters

The transcript highlights an important failure mode: if reward scores are constant, the resulting advantages can be zero. When advantages are zero, the loss signal can become zero even if the policy and reference models are otherwise set up correctly. Useful reward functions need enough variation to distinguish better and worse completions.[^source]

## Relationship to GRPO Loss

In [[grpo-loss]], advantages provide the core learning signal, especially at the start of training. When the policy model and reference model are identical, the probability ratio is one, so the loss depends directly on the advantage. As training proceeds and the policy changes, the ratio and advantage jointly scale updates.[^source]

## Related Pages

- [[topics/reinforcement-learning/wiki/reward-vs-return|reward-vs-return]]
- [[grpo-loss]]
- [[ratio-clipping]]
- [[fine-tuning-map]]

## Sources

[^source]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
