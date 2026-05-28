# GRPO Loss

GRPO loss is the training objective used to update a [[policy-and-reference-models|policy model]] from reward-weighted sampled completions while controlling unstable updates and drift from a frozen reference model.

The transcript breaks the GRPO loss into four practical components: policy probability ratio, [[advantages|advantage]] weighting, [[ratio-clipping|ratio clipping]], and a [[kl-divergence-penalty|KL divergence penalty]]. The loss is computed only over generated completion tokens, usually with a [[completion-mask|completion mask]].[^source]

## Core Intuition

GRPO increases the probability of completion tokens that led to positive advantages and decreases the probability of tokens that led to negative advantages. Because optimizers minimize loss, the reward-maximizing objective is typically sign-flipped into a minimization objective.[^source]

The policy ratio compares how likely the trainable policy model is to produce each completion token relative to the reference model. In probability form this is:

```text
policy_probability / reference_probability
```

In implementation, the same ratio is usually computed from log probabilities:

```text
exp(policy_logprob - reference_logprob)
```

This tells the trainer whether the trainable model has become more or less confident than the frozen reference model on the actual generated tokens.[^source]

## Token-Level Calculation

The transcript describes the implementation flow as:

1. Tokenize the prompt and completion.
2. Concatenate them into one input tensor.
3. Build an attention mask for the model forward pass.
4. Build a completion mask so only generated tokens contribute to loss.
5. Compute per-token log probabilities from the policy model and reference model.
6. Convert log probability differences into per-token ratios.
7. Multiply ratios by the completion advantage.
8. Apply ratio clipping.
9. Add the KL penalty.
10. Average or sum over completion tokens only.[^source]

## First-Step Behavior

At the first training step, the policy model and reference model may be identical, so the ratio is one. The transcript emphasizes that learning can still start because the advantage term supplies the gradient signal. If rewards are constant across sampled outputs, advantages become zero and the loss can collapse to zero, preventing useful learning.[^source]

## Related Pages

- [[advantages]]
- [[policy-and-reference-models]]
- [[completion-mask]]
- [[ratio-clipping]]
- [[kl-divergence-penalty]]
- [[fine-tuning-map]]

## Sources

[^source]: Raw transcript: `../raw/courses/reinforcement_finetuning_llms_with_grpo/calculating-loss-in-grpo.md`.
