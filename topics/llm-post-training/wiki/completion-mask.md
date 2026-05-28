# Completion Mask

A completion mask selects which tokens should contribute to the training loss. For instruction-style LLM fine-tuning, this usually means ignoring prompt tokens and computing loss only on the generated or target completion tokens.

In the GRPO transcript, prompt tokens and completion tokens are concatenated into one tensor before being passed to the model. The completion mask is built as zeros over prompt-token positions and ones over completion-token positions. Multiplying loss by this mask removes prompt-token loss from the final objective.[^source]

## Why It Matters

GRPO trains from sampled completions and their rewards. The reward applies to the generated answer, not to the input prompt. A completion mask keeps the [[grpo-loss|GRPO loss]] focused on the tokens whose probabilities should be updated because of the reward signal.[^source]

This mirrors a broader post-training pattern: in supervised fine-tuning and RL-style fine-tuning, the model often receives the full prompt as context, but only assistant or completion tokens are treated as training targets.

## Related Pages

- [[grpo-loss]]
- [[advantages]]
- [[fine-tuning-map]]

## Sources

[^source]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
