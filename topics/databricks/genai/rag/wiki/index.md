# RAG – Retrieval Augmented Generation (Databricks)

This topic covers the architecture and engineering discipline behind Retrieval Augmented Generation on Databricks, from the limits of prompt engineering through to production-grade context management.

## Map

- [[prompt-engineering|prompt-engineering]]
- [[retrieval-augmented-generation|retrieval-augmented-generation]]
- [[context-engineering|context-engineering]]
- [[token-budgets-and-context-windows|token-budgets-and-context-windows]]

## Core Concepts

- Prompt engineering vs. context engineering
- Knowledge cutoff, hallucination, and ambiguity as failure modes of prompting
- RAG: retrieval → augmentation → generation
- Context rot: context poisoning and "lost in the middle"
- System prompt design as a behavioral program
- Grounding instructions and metadata filtering
- Multi-turn state management (summarization, moving window, selective persistence)
- Token budgets, input/output token costs
- Just-in-time retrieval and reranking

## Learning Path

- Start with [[prompt-engineering]] to understand what prompts can and cannot do, and why RAG is necessary.
- Read [[retrieval-augmented-generation]] for the core architectural pattern and the new challenges it introduces.
- Read [[context-engineering]] to learn how to structure the context window for production reliability.
- Finish with [[token-budgets-and-context-windows]] for the economics and optimization strategies.

## Source Queue

Add course notes, papers, and personal notes to `../raw/`, then ingest them into this wiki.

## Sources

- [[../raw/courses/databricks-partner-academy/beyond-prompts-retrieval-agents-and-context-engineering|Beyond Prompts – Retrieval Agents and Context Engineering]] — Databricks Partner Academy, 2026.
