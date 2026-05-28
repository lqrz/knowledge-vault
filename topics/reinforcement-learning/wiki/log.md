# Log

## 2026-05-18

- Moved `reward-vs-return.md` into this topic from `llm-post-training` as the canonical page for the general RL distinction between immediate reward and accumulated return.
- Created the initial `reinforcement-learning` topic structure with `raw/` and `wiki/`.
- Added `index.md`, `overview.md`, `glossary.md`, `open-questions.md`, and this maintenance log.
- Defined the topic as a shared reference layer for general reinforcement learning concepts used by machine learning, deep learning, and LLM post-training notes.
- Added Sutton and Barto's Monte Carlo Methods chapter to `raw/books/sutton-barto-reinforcement-learning/monte-carlo-methods.pdf`.
- Ingested the Monte Carlo Methods chapter into focused wiki pages: `monte-carlo-methods.md`, `monte-carlo-prediction.md`, `monte-carlo-action-values.md`, `monte-carlo-control.md`, `exploring-starts.md`, `on-policy-monte-carlo-control.md`, `off-policy-monte-carlo.md`, and `importance-sampling.md`.
- Updated the reinforcement-learning index, glossary, and open questions with Monte Carlo method concepts and links.
- Added `prediction-and-control.md` to make the general RL distinction between policy evaluation and policy improvement reusable outside the Monte Carlo-specific pages.
- Expanded `monte-carlo-methods.md` with the reason Monte Carlo state estimates are independent while dynamic-programming estimates are coupled through Bellman backups.
- Added Sutton and Barto's Temporal-Difference Learning chapter to `raw/books/sutton-barto-reinforcement-learning/temporal-difference-methods.pdf` and extracted `temporal-difference-methods.txt` with `pdftotext` for synthesis.
- Ingested the Temporal-Difference Learning chapter into focused wiki pages: `temporal-difference-learning.md`, `td-prediction.md`, `td-error.md`, `sarsa.md`, `q-learning.md`, `expected-sarsa.md`, `maximization-bias.md`, `double-q-learning.md`, and `afterstates.md`.
- Updated the reinforcement-learning index, glossary, open questions, and Monte Carlo overview with temporal-difference learning concepts and links.
- Reorganized method-family wiki pages into `wiki/monte-carlo-methods/` and `wiki/temporal-difference-methods/`, keeping shared topic pages such as `index.md`, `overview.md`, `glossary.md`, `prediction-and-control.md`, `reward-vs-return.md`, `open-questions.md`, and `log.md` at the wiki root.
- Rewrote internal wikilinks to point to the new Monte Carlo and temporal-difference method folders.
