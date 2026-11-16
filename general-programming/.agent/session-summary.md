<!-- AI-oriented handoff checkpoint: keep this file current and free of secrets. -->

# Agent Session Summary

## Current state

- Task: Improve the `general-programming` skill with persistent agent-session handoffs and mandatory stack-appropriate `.gitignore` and `.dockerignore` files; distribute it to Codex and Claude global skill directories.
- Status: Workspace package updated and copied to both global skill directories; final exact-tree verification completed.
- Source package: `C:\Users\ranul\Documents\GitHub\ai-skills\general-programming`
- Target packages: `C:\Users\ranul\.codex\skills\general-programming` and `C:\Users\ranul\.claude\skills\general-programming`

## Completed actions

- Added instructions requiring agents to read, maintain, and update a persistent project session summary, preferring `.agent/session-summary.md`.
- Added requirements to create and tailor both `.gitignore` and `.dockerignore` for every project without hiding intended source, tests, documentation, or handoff notes.
- Updated the skill workflow and completion checklist to include the new artifacts and handoff update.
- Updated the package README with the new behavior.
- Added package-level `.gitignore` and `.dockerignore` suitable for Markdown/YAML documentation plus common validation tooling.

## Validation and next action

- Structural validation passed: required files exist and `SKILL.md` front matter contains the expected name and description.
- Global-copy validation passed: both target trees contain the same source files with matching SHA-256 hashes and no unintended nested directories.
- The bundled Python validator was not runnable because this environment has no `python`, `py`, or `python3` command.
- The workspace root is not a Git repository, so Git status was not applicable.
- Next action: future agents should read this summary first and use the updated skill behavior for project work.
