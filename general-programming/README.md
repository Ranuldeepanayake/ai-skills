# General Programming Skill

## Purpose

This portable skill gives Codex and Claude a consistent workflow for general programming in any language. It emphasizes requirements analysis, maintainable and modular code, clear naming, developer-facing comments, universal structured logging, comprehensive documentation, regression-safe testing, containerization, safe Git behavior, shell portability, and honest validation.

## Contents

- `SKILL.md` — the skill definition and operating procedure.
- `agents/openai.yaml` — optional Codex UI metadata; Claude does not require it.
- `.agent/session-summary.md` — the persistent agent handoff record for this package; project agents should create the equivalent file in each project they modify.

The package is tool-agnostic. Agents adapt commands and syntax to the repository's language, framework, package manager, CI system, host operating system, shell, and deployment platform.

## Required behavior

The skill requires agents to:

- understand the requirements and objectives and analyze the existing project before starting work;
- synchronize with the latest remote changes before implementation;
- preserve staged and unstaged work, including conflict recovery through stash, pull, and stash-pop when required;
- keep code organized and modular while following language-specific project structure and naming conventions;
- add a purpose comment block to each created or substantially rewritten source file;
- document every function, method, and public type;
- explain important or complex logic with focused comments;
- implement one universal application logger for every executable project, with ISO timestamps, severity, service/module context, structured safe metadata, centralized redaction, and useful error context;
- emit safe startup diagnostics for configuration, dependency/provider initialization, and final readiness before accepting traffic, plus observable shutdown and failure events;
- test or otherwise verify logger record shape, startup secret redaction, and important request/provider metadata;
- maintain a README reflecting all changes, including features, bug fixes, usage, purpose, stack, logic, setup, deployment, and limitations;
- add or update tests for behavioral changes and check for regressions or newly introduced bugs;
- maintain a persistent, secret-safe agent session summary with actions, decisions, validation, blockers, and next steps;
- create and tailor both `.gitignore` and `.dockerignore` files for the project's technology stack, while keeping intended source, tests, and documentation trackable;
- provide Docker and Docker Compose support when appropriate;
- use commands appropriate to the host operating system and active shell;
- include timestamped, contextual, secret-safe logging; and
- never commit or switch branches without explicit user instruction.

## Installation/use

For Codex, place or register the `general-programming` directory in the Codex skills directory and make `SKILL.md` available to the agent. Keep `agents/openai.yaml` for Codex UI metadata and discovery.

For Claude, expose the same directory as a project skill or add the contents of `SKILL.md` to the relevant Claude project or agent skill configuration. Claude does not need `agents/openai.yaml`.

The skill expects a project-level `.agent/session-summary.md` (or the repository's existing equivalent). Read it at session start, update it during meaningful milestones, and update it before handoff or completion. It is an AI-oriented operational checkpoint and should not contain secrets.

## Universal logging baseline

Use the existing logging library when the project has one; otherwise create a centralized logger module rather than scattering direct console or print calls. Each record should include an ISO-8601 UTC timestamp, level, service name, module/component, event or operation, message, and safe structured metadata. Include request or correlation IDs, status, duration, provider/endpoint context, retry information, and result counts when relevant.

Startup logging should include safe configuration and environment diagnostics, dependency/provider connectivity results, and the final listener/readiness event before the application accepts traffic. Sanitize endpoints and log only credential-presence booleans for secrets. Never log passwords, tokens, API keys, authorization headers, cookies, connection strings with credentials, certificate contents, prompts, or sensitive payloads. Document log levels, redaction rules, startup events, collection/viewing commands, and troubleshooting guidance in the project README.

## Validation

The package is documentation-only and has no runtime dependencies. Validate it by checking that `SKILL.md` exists, its YAML front matter contains `name` and `description`, and the instructions are consistent with the target agent's skill-loading format. Before distribution, review the Git synchronization guidance against the organization's repository policy and authentication model.

## Limitations

The skill cannot infer a project's correct conventions without inspecting the repository. It also cannot guarantee that a remote pull, conflict resolution, deployment, container runtime, external service, or hardware integration will succeed. Agents must distinguish static checks and local tests from live environment validation and must report blockers instead of pretending those checks passed.
