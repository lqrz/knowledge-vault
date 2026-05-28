# Retrieval Augmented Generation (RAG)

RAG is an architectural pattern that solves the knowledge limitations of LLMs by injecting external, up-to-date data into the prompt at inference time. Instead of relying on what the model memorized during training, the system retrieves relevant facts and supplies them as context — the model then generates an answer grounded in that provided evidence.

> **RAG vs. Retrieval Agent:** RAG is the *architectural pattern* (retrieval + augmentation + generation), independent of any specific tooling. A *retrieval agent* is a concrete implementation that operationalizes this pattern — handling query routing, retrieval orchestration, and context assembly within a real system.

## The Three Stages

```
Query → [Retrieval] → [Augmentation] → [Generation] → Answer
```

1. **Retrieval** — The system searches a knowledge base (e.g., indexed via Mosaic AI Vector Search) and fetches the most relevant chunks of text for the user's query.
2. **Augmentation** — The retrieved chunks are injected into the context window alongside the user's query and system instructions.
3. **Generation** — The model produces an answer using only the injected data, not its internal weights alone.

On Databricks, the knowledge base is typically indexed using **Mosaic AI Vector Search**, with documents stored and governed via Unity Catalog.

## Why RAG Works

RAG directly addresses the three [[prompt-engineering#The Hard Limits of Prompting|hard limits of prompting]]:

| Failure Mode | RAG Solution |
|---|---|
| Knowledge cutoff | Retrieve from a continuously updated knowledge base |
| Hallucination | Ground the model on retrieved facts; instruct it to cite only provided context |
| Ambiguity | Retrieve organization-specific documents that disambiguate domain terms |

## Context Rot: The New Challenge RAG Introduces

Naively retrieving large volumes of documents and concatenating them into the prompt creates a new class of failure: **Context Rot**.

### Context Poisoning
Including irrelevant or conflicting chunks in the context window confuses the model. If retrieval precision is poor, the model may use wrong information with high confidence.

### Lost in the Middle
Models tend to weight information at the **beginning and end** of the context window more heavily than information in the middle. When long documents are pasted in bulk, critical facts buried in the middle are effectively invisible to the model.

These failure modes motivate the discipline of [[context-engineering|Context Engineering]] — the deliberate design of *what* goes into the context window, in *what order*, and *how much* of it.

## Related

- [[prompt-engineering]]
- [[context-engineering]]
- [[token-budgets-and-context-windows]]

## Sources

- [[../raw/courses/databricks-partner-academy/beyond-prompts-retrieval-agents-and-context-engineering|Beyond Prompts – Retrieval Agents and Context Engineering]] — Databricks Partner Academy, 2026.
