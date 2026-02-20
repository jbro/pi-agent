---
name: reviewing-pi-skills
description: Use when reviewing an existing Pi skill for quality, spec compliance, or token efficiency.
---

# Reviewing Pi Skills

Review a skill across four dimensions: spec compliance, Pi content rules, description quality,
and token efficiency. Output a structured report; offer an optimized rewrite when requested
or when the skill is verbose.

## Step 1: Load the Skill

Read `SKILL.md` and every companion file before making any judgements.

## Step 2: Spec Compliance

| Check | Outcome if failed |
|---|---|
| `name` present in frontmatter | **Blocker** — not loaded by Pi |
| `description` present in frontmatter | **Blocker** — not loaded by Pi |
| `name` lowercase + hyphens only (`^[a-z0-9][a-z0-9-]*[a-z0-9]$`) | Warning |
| `name` ≤64 chars | Warning |
| `name` matches directory name | Warning |
| `description` ≤1024 chars | Warning |

Fix blockers before anything else.

## Step 3: Pi Content Rules

```bash
grep -n "superpowers:\|@[a-zA-Z]\|TodoWrite\|EnterPlanMode\|\bSkill tool\b\|\bTask tool\b\|~/.claude\|\bCLAUDE\.md\b" \
  ~/.pi/agent/skills/<name>/SKILL.md
```

Expect zero output. Also check manually:
- Sibling file references use explicit `Read` instructions, not `@file`
- Cross-references use skill name only, no prefix
- Paths point to `~/.pi/agent/skills/`, not `~/.claude/skills/`

## Step 4: Description Quality

Score 1–5:

- **Starts with "Use when..."** — 5 = yes, 1 = no
- **Triggers not workflow** — 5 = symptoms/situations, 1 = process steps
- **Specific keywords** — 5 = tool names, error types, 1 = vague abstractions
- **Length** — 5 = ≤200 chars, 1 = >600 chars
- **Third person** — 5 = yes, 1 = first person

**The test:** Could an agent decide to load or skip this skill from the description alone?
If no — rewrite the description.

## Step 5: Token Efficiency

```bash
wc -w ~/.pi/agent/skills/<name>/SKILL.md
```

| Word count | Rating |
|---|---|
| < 200 | Lean |
| 200–500 | Good |
| 500–900 | Review for cuts |
| > 900 | Verbose — optimize |

**Waste patterns:** repetition across sections; prose lists (→ tables); multiple weak examples (keep one strong); thin sections (header + 1–2 sentences); obvious statements; companion-file content inlined in SKILL.md.

## Step 6: Optimization Pass

When verbose (>500 words) or user requests: identify top 3 savings with estimated word counts → show before/after for biggest win → get confirmation → apply all agreed cuts → recount.

Target: ≥20% reduction without losing actionable content.

## Report Format

```
## Skill Review: <name>

**Spec:** PASS | FAIL | WARNINGS
<list issues if any>

**Content rules:** CLEAN | <N> issues
<list forbidden patterns found>

**Description:** <score>/5
<one sentence on what to improve, or "strong">

**Token count:** <N> words — LEAN | GOOD | VERBOSE
<top 2–3 savings if VERBOSE>

**Recommendation:** ship as-is | minor fixes needed | optimize | rewrite
```
