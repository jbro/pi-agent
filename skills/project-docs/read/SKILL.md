---
name: read
description: Use at the start of any task in a project to load existing context from docs/ before responding or planning.
---

# Read Project Docs

1. List `docs/` in the project root.
2. Read every markdown file there (recurse into subdirectories; observe any project-specific exclusions in AGENTS.md).
3. Summarise what you learned in 2–3 sentences so the user can confirm or correct.

If `docs/` does not exist or is empty, say so and proceed normally.
