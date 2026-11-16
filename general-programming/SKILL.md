---
name: general-programming
description: A language-agnostic programming workflow for Codex and Claude that enforces requirements analysis, maintainable structure, naming, documentation, tests, containerization, safe Git operations, shell portability, and explicit validation. Use for creating, modifying, reviewing, debugging, or refactoring software in any programming language.
---

# General Programming Skill

Use this skill for software work in any language or framework. Adapt commands, syntax, and examples to the repository's language, framework, shell, build system, and deployment target.

## Operating principles

1. Understand the work before editing. Read the project requirements and objectives, analyze the work already completed, and identify existing behavior and conventions. Do not begin implementation while key requirements remain unclear.
2. Inspect the repository root, language/toolchain, build and test commands, deployment targets, and current Git state.
3. Preserve user work. Never overwrite unrelated changes, discard files, or use destructive Git commands without explicit authorization.
4. Do not commit or switch branches unless the user explicitly asks. Staging, committing, and changing branches are separate actions.
5. Keep code well-organized and modular, with responsibilities separated according to the chosen language and framework's best practices.
6. Log troubleshooting information with timestamps, module/component names, severity, and useful context. Never log secrets, tokens, passwords, or sensitive payloads.
7. Maintain a persistent agent session summary in the project so another agent can quickly resume after a new session or context reset. Keep it concise but technically useful, and include completed actions, current state, decisions, assumptions, files changed, validation results, blockers, and the next recommended action. Update it after meaningful work and before handing off; never put secrets or raw credentials in it.

## Host shell and command portability

Use commands appropriate for the host operating system and active shell. On Windows, use PowerShell or Command Prompt syntax and path conventions. On Linux or macOS, use Bash or Zsh syntax and path conventions. When command behavior differs between shells, state which shell the command targets.

## Required pre-work Git synchronization

Before implementation work, fetch and pull the latest changes from the configured remote:

1. Inspect status, current branch, remotes, and upstream tracking.
2. Fetch the configured remote and pull the latest upstream changes.
3. If the pull reports conflicts because of local staged or unstaged work, stop and record the conflict state, stash both staged and unstaged changes (including untracked files when needed), pull again, then pop the stash.
4. Resolve resulting conflicts carefully while preserving the user's local work. Show ambiguous resolutions to the user before continuing.

Do not silently reset, clean, force-push, delete local work, or switch branches. If authentication, network access, a dirty repository, or a conflict prevents safe synchronization, report the exact blocker and ask for direction when needed.

## Project structure, naming, and tracking

Use the chosen language and framework's best-practice directory structure. Follow the nearest existing project pattern; for a new project, choose a conventional structure and document it in the README. Keep application code, configuration, tests, documentation, scripts, deployment files, and generated artifacts appropriately separated.

Use standard naming conventions for files, directories, modules, classes, functions, variables, constants, tests, and environment variables. Preserve a consistent existing convention unless migration is requested. Names must communicate intent and avoid unexplained abbreviations.

Create or update both a language- and tool-appropriate `.gitignore` and `.dockerignore` for every project, even when a Dockerfile is not currently present. Tailor both files to the detected stack and repository layout: exclude build output, caches, IDE metadata, local environment files, dependencies, generated artifacts, secrets, and files that must not enter a Docker build context. Do not exclude source, tests, documentation, deployment definitions, or the persistent agent session summary unless the user explicitly requests it. Track every intended project file not excluded by `.gitignore`; review `git status` before completion so files are not accidentally left untracked.

## Persistent agent session summary

Create the summary at the nearest project-appropriate path, preferring `.agent/session-summary.md` when no existing convention applies. If the repository already has an agent handoff or context file, extend that file instead of creating a duplicate. The summary is an operational checkpoint for agents, not a replacement for the README or changelog.

At the start of a session, read the summary before making changes and verify its claims against the current repository state. During the task, update it after meaningful milestones. Before completion, record:

- the task and intended outcome;
- actions completed and files changed;
- architecture and implementation decisions, including small technical details useful for continuation;
- commands run and their results, clearly separating static/local checks from live provider, runtime, container, or hardware validation;
- unresolved issues, blockers, assumptions, and risks; and
- the exact next action for a future agent.

Use dated entries or a clearly maintained current-state section so the latest state is unambiguous. Keep the file safe to commit: redact secrets, tokens, credentials, private payloads, and unnecessary personal data. Add a short developer-facing comment at the top when the file is created explaining its purpose.

## Source-file and code comments

Every source file created or substantially rewritten must begin with a concise comment block explaining its purpose, responsibility, important inputs/outputs or side effects, and non-obvious constraints when applicable.

Document every function, method, public type, class, or equivalent callable with its purpose, key parameters, return value, side effects, and failure behavior using the language's idiomatic documentation format.

Add focused inline comments for important or complex logic, security decisions, concurrency, performance tradeoffs, non-obvious transformations, workarounds, protocol details, and tricky edge cases. Explain why, not merely what.

## README requirements

Maintain a comprehensive README for every project. It must cover purpose and scope, use cases and non-goals, architecture and logic/data flow, tech stack and versions, prerequisites, configuration, local setup, build/lint/format/test commands, deployment and rollback instructions, Docker/Compose usage, logging and troubleshooting, security and secret management, limitations, assumptions, compatibility, known failure modes, and maintenance guidance when relevant.

Update the README in the same change as every feature, bug fix, behavior, setup, configuration, deployment, or limitation change. Include usage instructions and runnable commands.

## Testing and regression prevention

Write or update unit tests whenever behavior is added, changed, fixed, or made regression-prone. Every codebase change must be tested at an appropriate level and reviewed for regressions and newly introduced bugs. Cover normal behavior, validation, error paths, boundaries, and important security or data-integrity rules. Add integration or end-to-end tests when unit tests cannot verify an important boundary.

Run narrow tests first, then the broader suite when practical. Run formatting, linting, type checks, and builds as appropriate. Report commands, results, and environment-dependent checks that could not run. Do not weaken tests merely to make them pass, and do not claim live service, cloud, container, or hardware validation from static tests alone.

## Containerization

For deployable applications, provide containerization when appropriate:

- a production-appropriate, reproducible Dockerfile;
- the project-level `.dockerignore` maintained by the workflow, excluding secrets, dependencies, build output, and local files from any Docker build context;
- Docker Compose for local multi-service development when multiple services or dependencies are required;
- non-root execution, health checks, explicit ports/volumes/dependencies, and managed versions where meaningful;
- configuration through environment variables or secret mechanisms, never baked-in credentials; and
- README instructions for build, run, logs, tests, persistence, cleanup, and limitations.

## Logging and error handling

Use the project's standard structured logger where available. Useful events should include an ISO-8601 timestamp, severity, module/component, operation name, and safe context such as request or job identifiers. Preserve actionable error context, avoid swallowing failures, and do not expose secrets or sensitive internals.

## Implementation workflow

1. Synchronize safely with the remote using the pre-work Git procedure.
2. Read requirements and objectives, analyze existing project work, and establish conventions and current behavior.
3. State the intended change, assumptions, affected files, and validation plan.
4. Design the smallest well-organized, modular change that preserves existing behavior.
5. Create or update source files with required headers, callable documentation, and focused logic comments.
6. Create or update the persistent agent session summary and record the intended change, assumptions, and validation plan.
7. Add or update both `.gitignore` and `.dockerignore` for the detected technology stack and repository layout.
8. Add or update tests and container/deployment assets as applicable.
9. Update the README with feature, bug-fix, usage, setup, deployment, and limitation changes.
10. Run appropriate validation and review results for regressions or newly introduced bugs.
11. Update the session summary with completed actions, validation results, blockers, and the next action.
12. Review the diff for accidental changes, secrets, missing comments, naming inconsistencies, documentation drift, and intended files left untracked.
13. Report changes, validation results, environment-dependent limitations, and current Git status.

## Completion checklist

- [ ] Requirements, objectives, and existing project work were analyzed before implementation.
- [ ] Latest remote changes were fetched and pulled, or the blocker was reported.
- [ ] Existing local work was preserved; no unauthorized branch switch or commit occurred.
- [ ] Structure is language/framework appropriate, modular, and well-organized.
- [ ] Names follow language and repository conventions.
- [ ] A persistent, secret-safe agent session summary exists, was read at start, and was updated before completion.
- [ ] New or substantially rewritten source files have purpose headers.
- [ ] Functions, methods, and public types are documented.
- [ ] Complex or important logic has accurate explanatory comments.
- [ ] README reflects all changes and covers purpose, stack, logic, setup, deployment, usage, and limitations.
- [ ] Relevant tests were added or updated, run, and reviewed for regressions.
- [ ] `.gitignore` and `.dockerignore` exist, are tailored to the stack, and exclude secrets and irrelevant generated files without hiding intended source or documentation.
- [ ] Dockerfile and Compose support were added or updated when applicable.
- [ ] Commands match the host operating system and shell.
- [ ] Logging includes timestamps, module/component context, and safe troubleshooting data.
- [ ] Validation results and remaining limitations were reported honestly.
