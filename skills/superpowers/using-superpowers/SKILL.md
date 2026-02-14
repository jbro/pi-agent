---
name: using-superpowers
description: Use when starting any conversation. Enforces skill-first workflow discipline in Pi and requires loading relevant skills before responding, including clarifying questions.
---

<EXTREMELY-IMPORTANT>
If there is even a 1% chance a skill applies, you MUST load and use it.

If a skill applies, you do not have discretion to skip it.
</EXTREMELY-IMPORTANT>

# Using Skills in Pi

## How to Access Skills

In Pi, load skills by reading their `SKILL.md` files from `~/.pi/agent/skills/` (or other configured skill directories).

If you are using the superpowers port, start from `superpowers/SKILL.md` and follow its compatibility mapping.

## The Rule

**Load relevant or requested skills BEFORE any response or action.**

Even if you only need to ask a clarifying question, check skills first.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Load brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Read relevant SKILL.md" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create/update explicit checklist" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "Already brainstormed?" -> "Load brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Load brainstorming skill" -> "Might any skill apply?";

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Read relevant SKILL.md" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Read relevant SKILL.md" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "Create/update explicit checklist" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create/update explicit checklist" -> "Follow skill exactly";
}
```

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is a simple question" | Questions are tasks. Check for skills. |
| "I need context first" | Skill check comes before clarifying questions. |
| "I can inspect files quickly first" | Skills define how to inspect. Check first. |
| "I remember the skill" | Skills change. Read current text. |
| "This is overkill" | Discipline prevents rework. Use the skill. |

## Skill Priority

When multiple skills may apply:

1. **Process skills first** (brainstorming, debugging)
2. **Implementation skills second** (domain-specific execution)

## User Instructions

User instructions say **what** to do. Skills define **how** to do it.
