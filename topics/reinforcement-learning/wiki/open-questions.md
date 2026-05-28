# Reinforcement Learning Open Questions

Use this page to track unresolved questions, contradictions between sources, and claims that need stronger citations before becoming stable concept pages.

## Questions

- Sutton and Barto now define the initial notation for Monte Carlo methods. Should the broader topic use their notation for all tabular RL pages, or should later pages introduce a notation compatibility table?
- Should imitation learning live inside this topic, or only be linked when it directly supports reinforcement learning notes?
- Where should the boundary sit between general RL reward modeling and LLM-specific reward modeling in [[topics/llm-post-training/wiki/index|llm-post-training]]?
- How much detail should be split out for ordinary versus weighted importance sampling before function-approximation sources are ingested?
- Should the TD pages introduce a separate page for batch TD and certainty-equivalence estimates, or is the summary in [[temporal-difference-methods/prediction|td-prediction]] sufficient until planning/model-based chapters are ingested?
- Should [[temporal-difference-methods/expected-sarsa|expected-sarsa]] be treated as a mainline control page despite being less commonly named than [[temporal-difference-methods/sarsa|sarsa]] and [[temporal-difference-methods/q-learning|q-learning]] in later applied sources?

## Sources

- [[AGENTS|LLM Research Vault Instructions]]
- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 5, Monte Carlo Methods]]
- [[topics/reinforcement-learning/raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf|Sutton and Barto, Reinforcement Learning: An Introduction, Chapter 6, Temporal-Difference Learning]]
