---
name: project-context
class: discipline
description: >-
  Maintain an AI-readable knowledge layer for the project inside .agents/context/
  so agents understand the codebase without re-exploring it every session. Use
  when starting work on a project with no .agents/context (bootstrap), before
  substantial project work (read context first), after significant changes to
  architecture, domains, APIs, data models, or workflows, or when the user asks
  to update, audit, or initialize the project context.
---

# Project Context

Maintain structured, AI-readable project context in `.agents/context/`. The context gives coding agents reliable high-level understanding of a project without repeated full-codebase exploration. It is a **knowledge layer derived from the actual project** — never a replacement for verifying the project itself.

## Iron Rules

**Source of truth is always the actual project**, in this order: source code, database/schema, configuration, infrastructure/deployment configuration, tests and other executable project artifacts. Context is derived knowledge.

If context contradicts the actual project:

1. Verify the actual project.
2. Treat the project as authoritative.
3. Correct the outdated context if appropriate.
4. Continue the requested task.

Never modify source code merely to make it agree with outdated context. When information conflicts: **actual project > config.md > existing context > framework defaults**.

## Core Responsibilities

1. **Bootstrap** — create the context system when missing; analyze the project; generate appropriate files.
2. **Understand** — read context before performing project work; use it to reduce unnecessary exploration.
3. **Maintain** — detect changes that affect project knowledge; act according to the configured update mode.
4. **Synchronize** — keep context consistent with the actual project; correct outdated or inaccurate context when discovered.

## Context Layout

```text
.agents/context/
├── README.md      # entry point: what exists here, how an agent should use it, which files to read per task
├── config.md      # update mode, detail level, scope, template, other maintenance preferences
├── structure.md   # specification of the context itself: files, responsibilities, creation rules, detail levels
└── domains/       # optional: per-domain files (authentication.md, orders.md, ...)
```

- `README.md` must explain what the directory contains, how an AI agent should use it, which files exist, which files to read for common tasks, and navigation information. Keep it concise; it never duplicates the whole context.
- `config.md` defines how the context system behaves: update mode, detail level, scope, template, and other project-specific preferences.
- `structure.md` is the **context structure specification**: which files exist, what each is responsible for, when a file is created, what belongs and does not belong in each file, how structure differs per detail level, and domain organization where applicable. It lives inside the context and is therefore itself maintained.

## Startup Check

Check whether `.agents/context/` exists.

- **Exists:** read `README.md`, `config.md`, `structure.md`; follow the configured rules. Never recreate or destroy existing context because it differs from a preferred default. Improve incomplete files only when necessary.
- **Missing:** bootstrap it — see [bootstrap-and-config.md](./references/bootstrap-and-config.md). Ask for update mode, detail level, and scope preferences during bootstrap; persist them in `config.md`.

## Read Before Tasks

Recommended order:

```text
README.md → config.md → structure.md → relevant context files → relevant source code
```

Do not blindly read every context file on every task. Use `README.md` and `structure.md` to determine relevance:

- Database task → `database.md`, `architecture.md`, relevant domain context
- API task → `api.md`, `architecture.md`, relevant domain context
- UI task → frontend architecture, conventions, relevant domain context

## After Every Substantial Task

Ask internally: **did this change alter persistent project knowledge?**

- No (variable renames, formatting, typo fixes, minor CSS, small internal refactors, non-behavioral bug fixes): do not update context. These are implementation details, not persistent knowledge.
- Yes: classify the change significance (Level 0–3) per [change-significance.md](./references/change-significance.md), then follow the configured update mode per [context-updates.md](./references/context-updates.md).

Final checks:

- Did the organization of the context change? → update `structure.md`.
- Did navigation change? → update `README.md`.

## User Requests Override Detection

- "update the context" → update it regardless of the detected change level.
- "do not update context" → do not update for the current task unless required for safety or correctness of the task itself.
- "audit the context" → run the audit per [context-audit.md](./references/context-audit.md).

## Quality Rules

Context must be:

- **Accurate** — never intentionally document information known to be false.
- **Current** — remove or update information that is no longer true.
- **Concise** — avoid unnecessary explanations.
- **Discoverable** — an AI should quickly find what it needs.
- **Stable** — avoid documenting temporary implementation details.
- **Project-specific** — capture knowledge specific to this project; do not fill context with generic framework documentation.

## Do Not Over-Document

Never turn the context into a copy of the codebase. Do not document every class, function, endpoint, database column, or component unless the configured structure explicitly requires it and the information matters for AI-assisted development.

Prefer why / what / relationships / rules / workflows / important constraints over complete implementation inventories.

Content expectations per detail level: [detail-levels.md](./references/detail-levels.md).

## Principle

The goal is not more documentation. The goal is **better AI memory**: the smallest amount of reliable information that lets a new agent understand the project quickly and make correct changes without unnecessary exploration.
