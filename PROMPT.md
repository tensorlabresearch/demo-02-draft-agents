# Demo Prompt

Paste this into Claude Code to run the demo:

```
Read the docs/ folder and write an AGENTS.md for this project.
```

## What to watch for

Claude should read all three docs before writing anything:
- `docs/architecture.md` — system overview and components
- `docs/domain-glossary.md` — key terms and invariants
- `docs/testing-style.md` — how tests are written

A good AGENTS.md will synthesize these into a concise, actionable guide: project shape,
build/test commands, coding conventions, and done definition. It should feel like
something a new agent (or engineer) could open and immediately act on.

This demo shows how rich context docs produce a much better AGENTS.md than asking
Claude to write one from scratch with no background material.
