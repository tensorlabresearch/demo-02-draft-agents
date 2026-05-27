# Demo 02 — Draft AGENTS.md from Docs

**Concept:** Good context in = good agent behavior out.

## What this teaches

An agent is only as good as the context it's given. This demo shows how to bootstrap
an `AGENTS.md` by pointing Claude at existing documentation. The output is a compact,
actionable guide that shapes every subsequent agent session in the project.

## The codebase

A fictional data pipeline service called **Nearline**. No source code — just three
documentation files that describe the system in enough detail for an agent to understand
what it's working with.

```
docs/architecture.md      — system overview: Ingester, Transformer, Writer, DLQ
docs/domain-glossary.md   — key terms, invariants, and naming conventions
docs/testing-style.md     — how tests are structured and what they cover
PROMPT.md                 — the prompt to give Claude
```

## Running the demo

Open Claude Code in this directory and paste the prompt from `PROMPT.md`.

No code to run — the output is a new `AGENTS.md` file.

## Expected behavior

Claude should read all three docs before writing anything. The resulting `AGENTS.md`
should include:
- A one-paragraph project description
- Build/test commands
- Key coding conventions drawn from the docs
- A done definition

Compare the quality against what you'd get by asking Claude to write an AGENTS.md
with no background material at all.
