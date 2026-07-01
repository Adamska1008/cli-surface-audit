---
name: cli-surface-audit
description: Use principles for judging and improving CLI command surfaces: command shape, overloaded flags, task flows, config models, help text, errors, output formats, shell completion, automation ergonomics, and agent-callable interfaces. Use when a CLI has become a parameter grab bag or needs a clearer progressive-disclosure design; includes a small safety floor for side-effecting commands as a secondary guardrail.
---

# CLI Surface Audit

Use this as a taste guide, not a script. A good CLI surface makes common work obvious, advanced work possible, automation stable, and dangerous work explicit. Apply judgment; do not mechanically emit every section below.

## Principles

- User-task shaped: commands follow what users are trying to do, not the internal modules or API objects.
- Progressive disclosure: the happy path is short; advanced flags exist but do not dominate first contact.
- Predictable grammar: verbs, nouns, flag names, aliases, output modes, and config precedence feel consistent across commands.
- Context over repetition: stable values live in project config, profiles, env vars, or init flows instead of being repeated as flags.
- Explicit modes: different behaviors are represented by clear verbs, subcommands, presets, or config objects, not tangled boolean flags.
- Safe by shape: preview, render, validate, or dry-run paths exist before submit/apply/delete/deploy style actions.
- Actionable feedback: help shows workflows and examples; errors explain what happened, why, and the next useful command.
- Composable output: human output is readable; machine output is stable via `--json`; logs/progress stay off stdout; exit codes mean something.
- Non-interactive ready: prompts can be bypassed with explicit flags; CI and agents do not hang unexpectedly.
- Evolvable surface: better names and shapes can be introduced with aliases, deprecations, and migration notes instead of needless breakage.
- Agent affordance: safe read-only commands are easy to discover, side-effecting commands are hard to call accidentally, and plan/validate outputs are deterministic.

## Judgment

When reviewing or redesigning, reason from the CLI's real domain. Ask:

- What are the top user jobs, and how many concepts must a user hold to do them?
- Which flags are true per-invocation choices, and which are project context pretending to be choices?
- Where does the CLI leak implementation detail instead of user language?
- Which commands are overloaded because one verb is carrying multiple workflows?
- Which outputs are for humans, scripts, logs, or agents, and are those audiences separated?
- Where would a first-time user get stuck, and where would a power user need more control?
- What small compatible change would remove the most cognitive load?

Use tables, examples, or proposed command sketches only when they clarify the judgment. Prefer a few sharp recommendations over a long checklist.

## Smells

- One command has many unrelated flags: split by workflow, mode, or resource.
- Many required flags repeat across commands: introduce config, profiles, or project context.
- Several booleans interact: make modes explicit.
- Help is a parameter dump: add examples, workflow grouping, and danger notes.
- Errors only say invalid: show expected shape and a likely fix.
- Human and machine output mix: separate stdout/stderr and add stable `--json`.
- Prompts block automation: add explicit non-interactive flags.
- Command names expose internals: rename around user-facing tasks.

## Safety Floor

Default to static review, docs, source, tests, and help output. Do not execute commands that may create, modify, delete, submit, deploy, publish, upload, sync, migrate, start or stop resources, contact production services, use real credentials, trigger billing, or write outside a temporary fixture unless the user explicitly approves.

Safe without approval: `--help`, `-h`, `help`, `--version`, `version`, `completion`, and clearly local read-only commands. If execution is unsafe, use source analysis or ask for command transcripts.
