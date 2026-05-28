# Overview

LLM post-training is the set of techniques used after pretraining to shape a model's behavior, capabilities, style, safety properties, and reliability.

The main stages usually include supervised fine-tuning, preference optimization or reinforcement learning, evaluation, and iterative data improvement.

## Current Mental Model

Pretraining teaches broad next-token prediction from large corpora. Post-training narrows and shapes that base model into something useful for instruction following, dialogue, reasoning, tool use, safety behavior, and domain-specific tasks.

## To Develop

- How supervised fine-tuning changes behavior.
- How preference data is collected and used.
- How reward models relate to RLHF.
- Why DPO-style methods avoid explicit reward model training.
- How GRPO-style methods are used in reasoning-oriented training.
- How evals guide post-training data and method choices.
