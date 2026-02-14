---
description: Build project context from docs/ at task start, and refine those docs when the user requests it (for example via /skill:project-docs and a "refine docs" instruction).
---

# Project Docs

Maintain a living knowledge base in each project's `docs/` directory.

## When to Use

- **Task start:** When beginning work in a project, read this skill and follow the "Build Context" steps to orient yourself.
- **Refine docs request:** When the user asks to refine docs (for example after invoking `/skill:project-docs`), follow the "Refine Docs" steps to capture what was learned.

> Note: `/refine-docs` in this skill is a mnemonic trigger, not a guaranteed built-in Pi slash command.

## Build Context (task start)

1. List the contents of `docs/` in the project root.
2. Read every markdown file found there (recurse into subdirectories).
3. Summarise what you learned in 2–3 sentences so the user can confirm or correct your understanding.
4. Use this context to inform your plan for the current task.

If `docs/` does not exist or is empty, say so and proceed normally.

## Refine Docs (user-triggered; `/refine-docs` shorthand)

After the session goal is achieved, distil what was learned back into `docs/`.

### Steps

1. Review the work done in this session — changes made, decisions taken, problems solved, patterns discovered.
2. Identify which existing doc files should be updated and what new information belongs in each.
3. Draft the edits (prefer small, surgical updates over rewrites).
4. **Before creating new files or changing the directory structure, propose the change and wait for approval.**
5. Apply approved changes.
6. Report what was updated, with file paths and a short summary.

### Guidelines

- Keep docs concise and scannable — headings, bullet points, short paragraphs.
- Record *why* decisions were made, not just *what* was done.
- Remove information that is now wrong or obsolete.
- Don't duplicate what is already clear from the code itself.
- One topic per file; split when a file covers unrelated concerns (ask first).
- **Writing quality:** Before writing or editing docs, load the `writing-clearly-and-concisely` skill and apply its rules.
