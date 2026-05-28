# Reinforcement Learning Overview

Reinforcement learning studies how an agent can learn behavior through interaction with an environment, using reward signals to improve decisions over time.

This topic is the vault's shared reference layer for general reinforcement learning concepts. Pages here should define reusable RL ideas without tying them only to robotics, games, recommender systems, or LLM post-training.

## Scope

Use this topic for broad reinforcement learning ideas:

- Problem formulations such as agents, environments, states, observations, actions, rewards, returns, episodes, trajectories, and Markov decision processes.
- Objective concepts such as expected return, discounting, value functions, action-value functions, advantages, and Bellman equations.
- Learning mechanics such as exploration, exploitation, temporal credit assignment, bootstrapping, dynamic programming, Monte Carlo estimation, temporal-difference learning, and policy improvement.
- Algorithm families such as Q-learning, SARSA, policy gradients, actor-critic methods, model-based RL, offline RL, and imitation-learning-adjacent methods when they are used as RL context.
- Evaluation concepts such as train-versus-evaluation policies, environment stochasticity, sample efficiency, off-policy evaluation, and reward misspecification.

Do not use this topic for neural-network implementation details that are not RL-specific; those belong in [[topics/deep-learning/wiki/index|deep-learning]]. Do not use it for general supervised-learning or evaluation terminology that is not specific to sequential decision-making; those belong in [[topics/machine-learning/wiki/index|machine-learning]]. Do not use it for LLM-specific alignment and post-training methods such as RLHF, RLAIF, GRPO, DPO, reward-model training, or instruction tuning except as cross-topic references; those belong in [[topics/llm-post-training/wiki/index|llm-post-training]].

## Cross-Topic Role

Other topic wikis should link back here when they rely on general RL terminology. For example, a post-training note about GRPO can cite future pages here on policies, advantages, returns, and policy-gradient objectives, while a deep-learning note about neural value-function approximation can cite future RL pages on value functions and temporal-difference targets.

## Maintenance Notes

- Create small, focused concept pages rather than one long reinforcement learning reference.
- Link related pages with Obsidian wikilinks.
- Cite raw source files or external sources when adding substantive claims.
- Update [[index]] and [[log]] after adding or changing pages.

## Sources

- [[AGENTS|LLM Research Vault Instructions]]
