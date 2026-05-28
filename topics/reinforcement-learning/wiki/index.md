# Reinforcement Learning

This topic covers reinforcement learning concepts, algorithms, and study notes that are broader than any one application domain.

Use this topic for durable RL foundations: agents, environments, states, actions, rewards, returns, policies, value functions, exploration, temporal credit assignment, Markov decision processes, dynamic programming, Monte Carlo methods, temporal-difference learning, policy gradients, actor-critic methods, offline RL, and evaluation.

LLM-specific reinforcement fine-tuning methods such as RLHF, RLAIF, PPO-style post-training, GRPO, reward modeling, and preference optimization belong in [[topics/llm-post-training/wiki/index|llm-post-training]], but they can link back here for general RL concepts.

## Map

- [[overview]]
- [[glossary]]
- [[reward-vs-return]]
- [[prediction-and-control]]
- [[monte-carlo-methods/overview|Monte Carlo methods]]
- [[temporal-difference-methods/overview|Temporal-difference methods]]
- [[open-questions]]
- [[log]]

### Monte Carlo Methods

- [[monte-carlo-methods/overview|Monte Carlo methods]]
- [[monte-carlo-methods/prediction|Monte Carlo prediction]]
- [[monte-carlo-methods/action-values|Monte Carlo action values]]
- [[monte-carlo-methods/control|Monte Carlo control]]
- [[monte-carlo-methods/exploring-starts|Exploring starts]]
- [[monte-carlo-methods/on-policy-control|On-policy Monte Carlo control]]
- [[monte-carlo-methods/off-policy-control|Off-policy Monte Carlo]]
- [[monte-carlo-methods/importance-sampling|Importance sampling]]

### Temporal-Difference Methods

- [[temporal-difference-methods/overview|Temporal-difference methods]]
- [[temporal-difference-methods/prediction|TD prediction]]
- [[temporal-difference-methods/td-error|TD error]]
- [[temporal-difference-methods/sarsa|Sarsa]]
- [[temporal-difference-methods/q-learning|Q-learning]]
- [[temporal-difference-methods/expected-sarsa|Expected Sarsa]]
- [[temporal-difference-methods/maximization-bias|Maximization bias]]
- [[temporal-difference-methods/double-q-learning|Double Q-learning]]
- [[temporal-difference-methods/afterstates|Afterstates]]

## Core Concepts

- Agent
- Environment
- State
- Observation
- Action
- [[reward-vs-return|Reward]]
- [[reward-vs-return|Return]]
- Discount factor
- Episode
- Trajectory
- Policy
- Value function
- Action-value function
- Advantage
- Markov decision process
- Bellman equation
- Exploration and exploitation
- Credit assignment
- Prediction
- Control
- Dynamic programming
- Monte Carlo methods
- Monte Carlo prediction
- Monte Carlo control
- First-visit Monte Carlo
- Every-visit Monte Carlo
- Exploring starts
- On-policy control
- Off-policy control
- Target policy
- Behavior policy
- Importance sampling
- Ordinary importance sampling
- Weighted importance sampling
- Temporal-difference learning
- TD prediction
- TD error
- TD(0)
- Q-learning
- Sarsa
- Expected Sarsa
- Maximization bias
- Double Q-learning
- Afterstate
- Policy gradients
- Actor-critic methods
- Model-based reinforcement learning
- Offline reinforcement learning
- Reward shaping
- Evaluation

## Learning Path

- Start with [[overview]] for the topic scope and its relationship to [[topics/machine-learning/wiki/index|machine-learning]], [[topics/deep-learning/wiki/index|deep-learning]], and [[topics/llm-post-training/wiki/index|llm-post-training]].
- Use [[glossary]] as the staging area for short RL definitions before promoting central terms into focused concept pages.
- Read [[prediction-and-control]] to distinguish policy evaluation from policy improvement.
- For sample-based episodic learning, start with [[monte-carlo-methods/overview|Monte Carlo methods]], then read [[monte-carlo-methods/prediction|Monte Carlo prediction]], [[monte-carlo-methods/action-values|Monte Carlo action values]], and [[monte-carlo-methods/control|Monte Carlo control]].
- Read [[monte-carlo-methods/exploring-starts|Exploring starts]], [[monte-carlo-methods/on-policy-control|On-policy Monte Carlo control]], and [[monte-carlo-methods/off-policy-control|Off-policy Monte Carlo]] to understand how Monte Carlo control handles exploration.
- Read [[monte-carlo-methods/importance-sampling|Importance sampling]] after [[monte-carlo-methods/off-policy-control|Off-policy Monte Carlo]] to understand how behavior-policy episodes can estimate target-policy values.
- For bootstrapped sample-based learning, start with [[temporal-difference-methods/overview|Temporal-difference methods]], then read [[temporal-difference-methods/prediction|TD prediction]] and [[temporal-difference-methods/td-error|TD error]].
- For one-step tabular TD control, read [[temporal-difference-methods/sarsa|Sarsa]], [[temporal-difference-methods/q-learning|Q-learning]], and [[temporal-difference-methods/expected-sarsa|Expected Sarsa]].
- Read [[temporal-difference-methods/maximization-bias|Maximization bias]] and [[temporal-difference-methods/double-q-learning|Double Q-learning]] after [[temporal-difference-methods/q-learning|Q-learning]] to understand a common overestimation failure mode and one standard correction.
- Add focused pages for Markov decision processes, value functions, Bellman equations, policy gradients, and actor-critic methods as raw sources are ingested.

## Source Queue

Add books, papers, course notes, articles, transcripts, repos, and personal notes to `../raw/`, then ingest them into this wiki.

## Sources

- [[AGENTS|LLM Research Vault Instructions]]
- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
