# Policy and Reference Models

In reinforcement-style LLM fine-tuning, the policy model is the model being trained, while the reference model is a frozen baseline used to measure and control how much the policy changes.

In the GRPO transcript, the reference model is the base Baby Llama model without LoRA weights and remains frozen throughout training. The policy model starts from the same base model but receives trainable LoRA adapters, such as LoRA modules inserted into attention projection layers.[^source]

## Why Use Both Models

The policy model explores new behavior and is updated from reward-weighted completions. The reference model provides a stable comparison point. GRPO uses the difference between their token probabilities to compute the [[grpo-loss|GRPO loss]] policy ratio and the [[kl-divergence-penalty|KL divergence penalty]].[^source]

If the policy assigns higher probability to a generated token than the reference, the policy ratio rises above one. If it assigns lower probability, the ratio falls below one. This comparison helps scale updates according to how far the trainable model has moved from the baseline.[^source]

## Adapter-Based Setup

The transcript presents a common parameter-efficient setup:

- Clone or reuse the base model as a frozen reference model.
- Add LoRA weights to the trainable policy model.
- Update only the LoRA weights during training.
- Compare policy and reference log probabilities on the same prompt-completion sequence.[^source]

## Related Pages

- [[grpo-loss]]
- [[ratio-clipping]]
- [[kl-divergence-penalty]]
- [[glossary]]

## Sources

[^source]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
