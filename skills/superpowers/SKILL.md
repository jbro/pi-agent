---
name: superpowers
description: Use when you want the obra/superpowers workflow in Pi. Loads the compatibility rules, then routes to specific superpowers skills for brainstorming, planning, TDD, debugging, execution, and review.
---

# Superpowers (Pi Port)

This is a Pi-compatible port of `obra/superpowers`.

## Start Here

1. Read `README.md` in this directory for compatibility rules.
2. Use `using-superpowers/SKILL.md` to enforce skill-first workflow discipline.
3. Load specific workflow skills as needed:
   - `brainstorming/SKILL.md`
   - `writing-plans/SKILL.md`
   - `executing-plans/SKILL.md`
   - `test-driven-development/SKILL.md`
   - `systematic-debugging/SKILL.md`
   - `requesting-code-review/SKILL.md`
   - `verification-before-completion/SKILL.md`

## Pi Compatibility Rules (mandatory)

- For any instruction that says to load a skill, read the relevant `SKILL.md` directly.
- For any instruction that says to track progress, keep an explicit checklist and update it continuously.
- For any instruction that says to delegate work:
  - use delegated workers when your Pi environment supports them, otherwise
  - run staged passes in one session (implement, spec-check, quality-check).
- For any instruction that says to switch into planning mode, produce a plan and wait for user approval before coding.

## Goal

Match upstream superpowers behavior as closely as possible while staying executable in Pi.
