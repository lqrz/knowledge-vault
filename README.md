# LLM Research Vault

A personal knowledge base for learning and working with large language models, maintained as an [Obsidian](https://obsidian.md) vault and processed by AI agents.

The vault separates **raw source material** (papers, docs, transcripts) from **synthesized wiki pages** — durable concept notes written for long-term reference. An AI agent reads sources in `raw/` and writes structured wiki pages in `wiki/`, following the rules in [`AGENTS.md`](AGENTS.md).

---

## Topics

| Topic | Description |
|-------|-------------|
| [LLM Post-Training](topics/llm-post-training/wiki/index.md) | How pretrained models are adapted via fine-tuning, RLHF, preference optimization, and evaluation |
| [Reinforcement Learning](topics/reinforcement-learning/wiki/index.md) | RL foundations: MDPs, policies, value functions, Monte Carlo, TD learning, policy gradients |
| [Machine Learning](topics/machine-learning/wiki/index.md) | General ML concepts: supervised/unsupervised learning, optimization, generalization, evaluation |
| [Deep Learning](topics/deep-learning/wiki/index.md) | Neural network foundations: linear algebra, transformations, single neurons and layers |
| [Statistics](topics/statistics/wiki/index.md) | Probability, information theory, and statistical concepts underpinning ML |
| [Anthropic Agent SDK](topics/anthropic/agent-sdk/wiki/index.md) | Claude Agent SDK: tools, MCP, sessions, subagents, permissions, hooks |
| [Databricks — RAG](topics/databricks/genai/rag/wiki/index.md) | Retrieval-Augmented Generation on Databricks: prompt engineering, context management, production patterns |

---

## Vault Structure

```
topics/
  <topic-name>/
    raw/       # Immutable source material (papers, docs, transcripts)
    wiki/      # Synthesized concept pages written by the agent
      index.md # Topic map and entry point
inbox/         # Drop new sources here before assigning to a topic
AGENTS.md      # Instructions for the AI agent that maintains this vault
```

---

## How to Add New Knowledge

1. Drop source material (PDF, markdown, URL transcript) into the relevant `topics/<topic>/raw/` folder, or into `inbox/` if unsorted.
2. Ask an AI agent to ingest it: *"Ingest the new source in `raw/` following `AGENTS.md`."*
3. The agent creates or updates wiki pages in `wiki/`, links related concepts, and updates the topic `index.md`.
4. Review the generated pages in Obsidian.

---

## How to Use This Vault

- **Browse by topic** — open any `wiki/index.md` above for a map of that topic's concepts.
- **Search in Obsidian** — use `Cmd+O` to open files or `Cmd+Shift+F` to full-text search.
- **Ask questions** — open a conversation with an AI agent and point it at the vault for answers grounded in your notes.
- **Study** — ask the agent to generate a study plan or quiz based on a topic's wiki pages.
