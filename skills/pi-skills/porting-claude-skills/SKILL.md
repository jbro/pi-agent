---
name: porting-claude-skills
description: Use when porting a Claude Code or superpowers skill to Pi/agentskills.io format. Covers frontmatter, tool references, cross-references, companion files, and subagent patterns.
---

# Porting Claude Skills to Pi

## Overview

Superpowers skills target Claude Code's `Skill` tool. Pi uses the agentskills.io standard:
skills live in named directories, Pi auto-discovers them at startup, injects metadata into
the system prompt as XML, and the agent loads full content on demand via the `Read` tool.

Most skills port with mechanical changes. A few need substantive adaptation. Two should be skipped.

## Portability Assessment

| Skill | Action | Reason |
|---|---|---|
| brainstorming | Direct port | No platform-specific tools |
| writing-plans | Direct port | Minor: remove subagent execution option |
| test-driven-development | Direct port | No platform-specific tools |
| systematic-debugging | Direct port | Copy companion files |
| verification-before-completion | Direct port | No changes needed |
| receiving-code-review | Direct port | No platform-specific tools |
| using-git-worktrees | Direct port | Minor: CLAUDE.md → AGENTS.md |
| finishing-a-development-branch | Direct port | Minor: CLAUDE.md → AGENTS.md |
| executing-plans | Direct port | Minor: remove subagent-driven option |
| using-superpowers | Adapt | Skill tool → Read tool; TodoWrite → checklist; EnterPlanMode → remove |
| requesting-code-review | Adapt | Task tool/subagent → direct review in current session |
| writing-skills | Adapt | Install dir; subagent testing → manual |
| subagent-driven-development | **Skip** | Entirely subagent-dependent |
| dispatching-parallel-agents | **Skip** | Entirely subagent-dependent |

## Transformation Rules

### 1. Frontmatter

```yaml
# Source (Claude)                    → Pi
name: some-skill                     # keep — must match directory name exactly
description: "..."                   # keep — trim to ≤1024 chars if needed
# drop any other fields
```

### 2. Cross-references

| Source pattern | Replace with |
|---|---|
| `superpowers:skill-name` | `skill-name` |
| `**REQUIRED SUB-SKILL:** Use superpowers:X` | `**See skill:** X` |
| Plain English skill references | Keep as-is |

### 3. Sibling file references

| Source pattern | Replace with |
|---|---|
| `@filename.md` | `\`filename.md\` in this skill's directory (load with Read tool)` |
| `See @filename.md for details` | `Read \`filename.md\` from this skill's directory` |

### 4. Platform-specific tools

| Source | Replace with |
|---|---|
| `Use the Skill tool` / `Invoke the skill` | `Read the skill's SKILL.md at its listed location` |
| `TodoWrite` item list | Plain `- [ ]` markdown checklist |
| `EnterPlanMode` | Remove; replace with "plan your approach before proceeding" |
| `Task` tool (subagent dispatch) | See Subagent section below |

### 5. Subagent dispatch (`Task` tool)

Pi has no built-in subagent dispatch. Replace based on usage:

- **Incidental dispatch** (e.g., "dispatch code-reviewer subagent"): replace with a direct
  instruction to perform that work in the current session.
- **Whole skill is subagent orchestration**: mark as **Skip** in the portability assessment.

### 6. Directory and file paths

| Source | Replace with |
|---|---|
| `~/.claude/skills/` | `~/.pi/agent/skills/` |
| `CLAUDE.md` references | `AGENTS.md` |

### 7. Companion files

| File type | Action |
|---|---|
| Reference docs (`root-cause-tracing.md`, etc.) | Copy verbatim |
| Scripts and tools (`find-polluter.sh`, etc.) | Copy verbatim |
| Subagent prompt templates (`*-prompt.md`, `code-reviewer.md`) | Skip |

## Steps

1. **Assess portability** — check the table; skip non-portable skills
2. **Read source** — `Read` SKILL.md and all companion files
3. **Create output directory**: `mkdir -p ~/.pi/agent/skills/<skill-name>/`
4. **Apply transforms** — frontmatter → cross-refs → sibling refs → tool refs → paths
5. **Copy companion files** that pass the filter above
6. **Write** adapted `SKILL.md`
7. **Verify**: `name` matches directory; no forbidden patterns remain (`grep` scan passes)
