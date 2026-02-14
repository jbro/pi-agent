# Superpowers for Pi

This directory ports [`obra/superpowers`](https://github.com/obra/superpowers) into Pi skills, with minimal changes.

## Source

- Upstream repo cloned at: `$HOME/tmp/superpowers`
- Upstream commit used: `e16d611`

## What was ported

- All upstream skill directories under `skills/`
- Supporting prompt/docs/script files used by those skills

## Pi compatibility mapping

Some upstream instructions target environment-specific primitives. In Pi, map them as follows:

- **Skill loading instructions** → read the referenced `SKILL.md` directly.
- **Checklist/progress tracking instructions** → maintain an explicit checklist in the response or in a plan/checklist file.
- **Delegation instructions** → use delegated workers only if available via extensions; otherwise emulate with staged passes in one session (implementer pass, spec-review pass, code-quality pass).
- **Planning-mode instructions** → use explicit plan-first workflow and wait for user approval before execution.

## Expected limitations

This is a close behavioral port, not a strict 1:1 runtime port. Any instruction that depends on unavailable tools must follow the compatibility mapping above.

## Recommended usage in Pi

1. Start with `/skill:superpowers` or `superpowers/using-superpowers`.
2. Load workflow skills as needed (`brainstorming`, `writing-plans`, `test-driven-development`, `systematic-debugging`, etc.).
3. Apply compatibility mapping when a skill references unavailable tool primitives.
