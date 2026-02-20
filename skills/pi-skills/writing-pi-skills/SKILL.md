---
name: writing-pi-skills
description: Use when creating or editing any Pi skill, or checking an existing skill's format for agentskills.io spec compliance and Pi content rules.
---

# Writing Pi Skills

Pi skills follow the [agentskills.io standard](https://agentskills.io/specification).
Pi auto-discovers skills, injects `name` + `description` into the system prompt at startup,
and loads the full `SKILL.md` on demand via the `Read` tool when a task matches.

## Frontmatter

Both fields are **required**. Missing either prevents the skill from loading.

```yaml
---
name: my-skill        # 1–64 chars; lowercase, digits, hyphens; matches directory name exactly
description: "..."    # ≤1024 chars; third person; triggering conditions only
---
```

**Name:** no uppercase, no consecutive hyphens (`--`), no leading/trailing hyphens.

```yaml
✅  name: systematic-debugging
✅  name: writing-pi-skills
❌  name: Systematic-Debugging   # uppercase
❌  name: my--skill              # consecutive hyphens
```

**Description:** describe *when* to load the skill — never summarize the skill's process or workflow.

```yaml
✅  Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
❌  Use for debugging — investigates root cause across 4 phases
❌  Helps with debugging.
```

## Directory Structure

```
~/.pi/agent/skills/<name>/
    SKILL.md              # required
    companion.md          # optional — heavy reference or reusable tools only
```

Pi scans `~/.pi/agent/skills/` and `.pi/skills/` (project-local) recursively.
Directory name must match `name`.

## Pi Content Rules

| ❌ Claude Code pattern | ✅ Pi replacement |
|---|---|
| `@filename.md` (auto-load) | `Read \`filename.md\` in this skill's directory` |
| `Skill` tool invocation | `Read the skill's SKILL.md at its listed location` |
| `TodoWrite` checklist | `- [ ]` markdown checklist |
| `EnterPlanMode` | "plan your approach before proceeding" |
| `superpowers:skill-name` | `skill-name` (name only) |
| `~/.claude/skills/` | `~/.pi/agent/skills/` |
| `CLAUDE.md` references | `AGENTS.md` |

Companion files: use for heavy reference (100+ lines) or reusable scripts. Pi has no auto-loading:

```markdown
✅  Read `api-reference.md` in this skill's directory for full API docs.
❌  @api-reference.md
```

## Token Efficiency

Only `name` + `description` are always in context; the full skill loads on demand.

| Skill type | Target |
|---|---|
| Workflow / always-triggered | < 200 words |
| Standard | < 500 words |
| Reference | No hard limit; compress where possible |

Prefer tables over prose, one strong example over three weak ones,
cross-reference rather than repeat, omit what any competent agent already knows.

## Checklist

- [ ] `name` present in frontmatter; lowercase, hyphens only, ≤64 chars; matches directory name exactly
- [ ] No leading/trailing/consecutive hyphens in `name`
- [ ] `description` present, ≤1024 chars, third person, starts with "Use when..."
- [ ] Description describes triggering conditions — not the skill's workflow
- [ ] No `@filename` references — sibling file loads are explicit `Read` instructions
- [ ] No `Skill tool`, `TodoWrite`, `EnterPlanMode`, `Task tool` references
- [ ] No `superpowers:` prefixes on cross-references
- [ ] No `~/.claude/` paths; no `CLAUDE.md` references
- [ ] Companion files are reference/tools only (not subagent prompt templates)
