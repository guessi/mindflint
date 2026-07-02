# AGENTS.md

## Repository purpose

This repository contains Mindflint, a Socratic guided learning skill suite.

Mindflint prevents fake learning by making the assistant ask first, guide derivation, verify understanding, and leave a takeaway question.

## Repository layout

* `skills/<skill-name>/SKILL.md`: reusable agent skills, shared by both hosts.
* `.claude-plugin/`: Claude Code plugin manifest and marketplace metadata.
* `.codex-plugin/plugin.json`: Codex plugin manifest.
* `.agents/plugins/marketplace.json`: Codex repo marketplace metadata.
* `README.md`: user-facing installation, usage, and examples.

## Compatibility policy

* Keep Claude Code support working.
* Keep Codex support additive; do not break existing Claude Code installation.
* Do not rename existing skills, commands, or plugin identifiers unless the README and marketplace metadata are updated together.
* The plugin name is `mindflint`.

## Skill authoring rules

* Skills live under `skills/<skill-name>/SKILL.md`.
* Each `SKILL.md` must keep YAML frontmatter with at least `name` and `description`.
* Keep descriptions concise, trigger-oriented, and clear.
* Prefer instruction-only skills unless scripts, hooks, or persistent storage are explicitly needed.
* The core Mindflint behavior is: set context first, guide derivation, verify understanding, then summarize key takeaways and prompt check/review.
* All skills respond in the same language the user writes in — no separate `-en` variants needed.

## Claude Code notes

* Claude Code plugin metadata lives in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`.
* Claude Code auto-discovers `skills/<name>/SKILL.md` at the plugin root; no separate command files are needed.

## Codex notes

* Codex plugin metadata lives in `.codex-plugin/plugin.json`.
* Codex marketplace metadata lives in `.agents/plugins/marketplace.json`.
* Codex should reuse the existing `skills/` directory rather than duplicating skill files.
* Codex support should remain an adapter layer around the existing Mindflint skills.

## Documentation rules

* Update `README.md` whenever installation steps, invocation syntax, supported hosts, or plugin names change.
* Keep examples aligned with the actual tested commands or skill invocation methods.
* When documenting host-specific behavior, clearly separate Claude Code usage from Codex usage.
