# Machine Learning Overview

Machine learning studies systems that improve task performance by fitting patterns from data rather than being programmed only through explicit rules.

This topic is the vault's shared reference layer for general machine learning concepts. Pages here should define concepts that other topics can reuse without duplicating definitions.

## Scope

Use this topic for broad machine learning ideas:

- Problem formulations such as supervised learning, unsupervised learning, reinforcement learning, classification, regression, ranking, and density estimation.
- Data concepts such as training data, validation data, test data, labels, targets, features, distributions, and sampling.
- Learning mechanics such as loss functions, empirical risk, optimization, regularization, and model selection.
- Evaluation concepts such as metrics, baselines, calibration, error analysis, and distribution shift.
- Generalization concepts such as overfitting, underfitting, inductive bias, and the bias-variance tradeoff.

Do not use this topic for material that is specific to neural network architectures, activation functions, or representation learning mechanics; those belong in [[topics/deep-learning/wiki/index|deep-learning]]. Use [[topics/reinforcement-learning/wiki/index|reinforcement-learning]] for focused notes on agents, environments, rewards, value functions, and sequential decision-making. Do not use this topic for LLM-specific adaptation methods such as RLHF, DPO, GRPO, reward modeling, or instruction tuning; those belong in [[topics/llm-post-training/wiki/index|llm-post-training]].

## Cross-Topic Role

Other topic wikis should link back here when they rely on general machine learning terminology. For example, a post-training note about reward model evaluation can cite a future machine-learning page on evaluation metrics, while a deep-learning note about training dynamics can cite future pages on optimization or generalization.

## Maintenance Notes

- Create small, focused concept pages rather than one long machine learning reference.
- Link related pages with Obsidian wikilinks.
- Cite raw source files or external sources when adding substantive claims.
- Update [[index]] and [[log]] after adding or changing pages.

## Sources

- [[AGENTS|LLM Research Vault Instructions]]
