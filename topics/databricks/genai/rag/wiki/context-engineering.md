# Context Engineering

Context Engineering is the strategic design of the **entire input window** supplied to a model. It is a shift from micro-optimization (crafting a better prompt) to macro-architecture (governing everything the model sees). The goal is to ensure the model has exactly the right signal — no more, no less — to perform reliably.

Where [[prompt-engineering|Prompt Engineering]] asks *"how do I phrase this instruction?"*, Context Engineering asks *"what should the complete system state look like at inference time?"*

The four elements being orchestrated are:
- **System Instructions** — behavioral program for the model
- **Conversation History** — relevant prior turns
- **Retrieved Data** — externally fetched, grounded facts
- **User Constraints** — current query, filters, and permissions

## Designing System Prompts

In a Context Engineering paradigm, the system prompt is not a casual instruction — it is a **behavioral program**. It should fully specify how the model must operate, leaving no room for the model to fill in defaults arbitrarily.

Three essential components:

### Role Definition
Explicitly define the model's persona. Generic personas produce generic behavior.

> *Example:* `"You are a Databricks Security Architect with expertise in Unity Catalog access control."`

A precise role primes the model to draw on the most relevant part of its pretrained knowledge.

### Negative Constraints
Specify what the model must not do. Omitting negative constraints allows the model to fall back on training defaults, which may be wrong for your use case.

> *Examples:*
> - `"Do not mention competitor products."`
> - `"Do not provide code unless the user explicitly requests it."`
> - `"Do not answer questions outside the scope of the retrieved context."`

### Output Formatting
Enforce a deterministic output structure. Downstream systems that parse model output need consistency.

> *Examples:* JSON, YAML, Markdown tables, structured field extraction.

Without format enforcement, the model will vary its output style across calls, breaking parsing logic.

## Grounding and Chunking

Retrieval alone doesn't prevent hallucination — the model must also be *instructed* to use only what was retrieved.

### Grounding Instructions
Explicitly constrain the model to the provided context chunks:

> *"Answer using ONLY the provided context chunks. If the answer is not present in the context, state: 'I do not have that information.' Do not use prior knowledge."*

Without this instruction, models will seamlessly blend retrieved facts with hallucinated ones.

### Metadata Filtering
Use **Unity Catalog metadata** to filter retrieval *before* chunks reach the model. This prevents the model from seeing stale or irrelevant data.

> *Example:* If a user asks about "2024 Revenue," filter vector search results to chunks where `year=2024`. Without this filter, the retriever may surface 2023 data and the model will use it.

Metadata filtering acts as a pre-retrieval access control and relevance layer, keeping the context window clean before grounding instructions even apply.

## Managing Multi-Turn State

In agentic applications, conversations grow across many turns. The context window is finite (see [[token-budgets-and-context-windows|token budgets]]), so history must be actively managed rather than naively accumulated.

Three strategies:

### Summarization
Periodically compress conversation history into a condensed summary of key decisions, facts established, and user intent. Replace the raw turn-by-turn log with the summary to free token space.

Trade-off: compression loses detail. Critical specifics (e.g., a file path the user specified three turns ago) must be identified and preserved before summarizing.

### Moving Window
Discard the oldest messages when the context fills up, keeping only the most recent N turns in full fidelity.

Works well when recent context is more relevant than early context (e.g., exploratory conversations). Breaks down when early turns established constraints that still govern current behavior.

### Selective Persistence
Identify which information must never be discarded (e.g., user name, current project ID, security permissions) and pin it to a protected region of the context. Everything else is subject to summarization or eviction.

This requires the system to classify context items by their shelf life at insertion time.

## The Shift from RAG to Context Engineering

| RAG | Context Engineering |
|---|---|
| Retrieves relevant chunks | Decides *which* chunks, *how many*, and *in what order* |
| Injects data into the prompt | Designs the full input environment: instructions + history + data + constraints |
| Solves the knowledge gap | Solves the reliability, accuracy, and cost gap |

RAG is a prerequisite; Context Engineering is what makes RAG production-grade.

## Related

- [[prompt-engineering]]
- [[retrieval-augmented-generation]]
- [[token-budgets-and-context-windows]]

## Sources

- [[../raw/courses/databricks-partner-academy/beyond-prompts-retrieval-agents-and-context-engineering|Beyond Prompts – Retrieval Agents and Context Engineering]] — Databricks Partner Academy, 2026.
