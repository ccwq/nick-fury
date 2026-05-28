# Progress

## 2026-05-28

- Reviewed Claude Code plugin / skills / subagents guidance for the proposed reuse model.
- Adopted `Skill 厚能力 + Agent 薄外壳` architecture:
  - `skills/` keeps role persona, workflow, checklists, and output templates as the main source of truth.
  - `agents/` registers native subagents with trigger descriptions, boundaries, and minimal persona anchors.
- Added 15 native role subagents under `agents/`.
- Updated `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` to expose commands, agents, and skills; added missing `./skills/nick` to marketplace skills.
- Updated `skills/nick-fury/SKILL.md` and `commands/nick-fury.md` to prefer native role subagents and fall back to `general-purpose` + role skill injection.
- Updated `README.md`, `docs/roles.md`, `task_plan.md`, and `findings.md` to reflect the new architecture.

## 2026-05-27

- Installed `Plugin Structure` skill into `.agents/skills/plugin-structure`.
- Investigated why Nick Fury did not actually distribute work to sub agents.
- Ran 5-round grill-me decision process with the user:
  - Chose smart dispatcher over forced dispatcher.
  - Chose explicit complexity thresholds.
  - Chose mixed parallel + serial dispatch.
  - Chose to modify Nick Fury + role skills, not add formal agents yet.
  - Chose behavioral validation.
- Updated `skills/nick-fury/SKILL.md` with Agent-first intelligent dispatch protocol.
- Updated `commands/nick-fury.md` so slash command requires actual sub agent tooling when dispatch criteria are met.
- Added “被分派任务协议” to role skills under `skills/`.
- Added project memory entry `nick-fury-smart-dispatch-preference.md`.
- Created PWF files: `task_plan.md`, `findings.md`, `progress.md`.

## Remaining

- Run `/reload-skills` before real behavior testing.
- Use validation prompts from `task_plan.md` to verify actual Agent calls.
- If behavior remains unstable, next escalation is adding formal `agents/` definitions or tightening command-level instructions further.
