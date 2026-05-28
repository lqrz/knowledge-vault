# LLM Research Vault Instructions

This vault is maintained as a compiled research wiki for learning and working with large language models.

## Core Rules

- Treat `raw/` folders as immutable source material.
- Put durable synthesis, explanations, maps, and study plans in `wiki/`.
- Every wiki page should cite the raw source files or external sources it depends on.
- Use Obsidian wikilinks such as `[[direct-preference-optimization]]`.
- Prefer small, focused concept pages over long catch-all notes.
- Update the relevant `index.md` after adding or changing wiki pages.
- Append meaningful maintenance actions to the relevant `log.md`.
- Mark uncertainty, contradictions, and stale claims clearly.

## Source Handling

When ingesting a new source:

1. Read the source from the relevant `raw/` folder.
2. Extract the durable concepts, claims, definitions, methods, and open questions.
3. Create or update wiki pages for those concepts.
4. Link related pages with Obsidian wikilinks.
5. Add citations back to the source.
6. Update the topic index and log.

When ingesting PDF documents, use `pdftotext` to extract the PDF text before synthesis. Treat the original PDF in `raw/` as the source of record, and cite it from the resulting wiki pages.

## Answering Questions

When answering questions about this vault:

1. Search the wiki first.
2. Search raw sources only when the wiki is missing detail or citations.
3. If the answer produces reusable insight, save it back into the wiki.
4. Keep direct answers concise, but preserve detailed synthesis in files.

## Linting

Periodically run a health check over the wiki to keep it consistent as it grows. A lint pass should cover:

1. **Broken wikilinks** — scan wiki pages for `[[...]]` links whose target file does not exist. Flag each broken link with the page it appears on and the missing target.
2. **Orphan pages** — find wiki pages that have no inbound links from any other page or index. Decide whether each orphan should be linked from a relevant page, merged into an existing page, or noted as a gap in the topic index.
3. **Index drift** — verify that every wiki page under a topic is listed in that topic's `index.md`, and that every link in `index.md` resolves to an existing file.
4. **Stale or contradicted claims** — look for pages that assert something a more recently ingested source has superseded or contradicted. Mark uncertain or outdated claims with a `> **Stale?**` blockquote and note the conflicting source.
5. **Concept stubs** — identify concepts that are mentioned or wikilinked across multiple pages but have no dedicated page of their own. Log these as candidates for the next ingest session.
6. **Missing citations** — flag wiki pages that make specific factual claims but cite no raw source. Either add a citation or add a `> **Unsourced.**` marker.
7. **Cross-topic consistency** — when the same concept appears in more than one topic (e.g. `importance-sampling` in both `reinforcement-learning` and `llm-post-training`), check that the two treatments are consistent and cross-linked.

After a lint pass, append a dated entry to the relevant `log.md` using the format `## [YYYY-MM-DD] lint` and list what was found and fixed.

## Current Topic Structure

- `topics/llm-post-training/`: supervised fine-tuning, preference optimization, RLHF, DPO, GRPO, reward models, evals, synthetic data, and post-training workflows.
