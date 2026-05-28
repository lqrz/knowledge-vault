# Token Budgets and Context Windows

Every LLM has a fixed **context window limit** — the hard upper bound on how many tokens it can process in a single call. This limit acts as working memory: everything the model can reason over at inference time must fit inside it. Context Engineering must operate within this constraint.

## Anatomy of the Context Window

The context window is consumed by two classes of tokens:

- **Input tokens** — everything sent to the model: system instructions, retrieved documents, conversation history, and the user's current query.
- **Output tokens** — the text the model generates in response.

Both are finite and both cost money.

### The "Lost in the Middle" Effect

Filling the context window to capacity degrades reasoning quality. Models weight information at the **beginning and end** of the context more heavily than information in the middle. As input token count grows:

1. Relevant facts buried in the middle become effectively invisible.
2. Latency increases significantly — processing a 128k-token context is far slower than a 4k one.
3. Cost scales linearly with token volume.

This creates a direct tension: more retrieved data means more knowledge available, but also more noise and worse reasoning.

## Token Economics

Databricks Foundation Model APIs (and most other providers) charge per token — input and output are typically priced separately. The cost of a naive RAG system compounds quickly:

> A system that retrieves 50 document chunks per query, with each chunk averaging 400 tokens, injects 20,000 tokens of context per call. At scale this burns through token budgets rapidly and produces diminishing returns as the "lost in the middle" effect kicks in.

Cost management is not a post-launch concern — it should be designed in from the start.

## Optimization Strategies

### Just-in-Time Retrieval
Rather than loading a large knowledge base upfront at the start of a conversation, give the agent a **retrieval tool** it can invoke on demand. The agent retrieves specific sections only when the user's query makes them relevant.

This keeps the context window lean for most turns and expands it only when necessary.

### Reranking
Retrieve a large candidate set (e.g., top 50 chunks by vector similarity) but then pass that set through a **reranker model** — a smaller model trained to score semantic relevance more precisely. Inject only the top 3–5 reranked chunks into the final context.

This two-stage approach separates recall (broad retrieval) from precision (tight selection), ensuring high-quality context without token waste.

| Strategy | What it solves |
|---|---|
| Just-in-time retrieval | Avoids loading irrelevant context preemptively |
| Reranking | Reduces injected chunk count without sacrificing recall |
| Metadata filtering (see [[context-engineering]]) | Removes irrelevant chunks before retrieval scores them |
| Summarization / moving window (see [[context-engineering]]) | Controls history growth across multi-turn conversations |

## Context Window Sizes (Reference)

Common context window sizes as of 2026:

| Size | Typical use |
|---|---|
| 8k tokens | Smaller open-source models |
| 32k tokens | Mid-range models |
| 128k+ tokens | Frontier models (GPT-4o, Claude 3.x, Gemini 1.5) |

Larger windows do not eliminate the need for token budget management — the "lost in the middle" effect and cost scaling apply regardless of window size.

## Key Principle

> **Treat the context window like a budget.** Every token should earn its place. Use filtering, reranking, and just-in-time retrieval to maximize the value of each token injected.

## Related

- [[retrieval-augmented-generation]]
- [[context-engineering]]

## Sources

- [[../raw/courses/databricks-partner-academy/beyond-prompts-retrieval-agents-and-context-engineering|Beyond Prompts – Retrieval Agents and Context Engineering]] — Databricks Partner Academy, 2026.
