# project-context Specification

## Intent

`project-context` is a `discipline`-class skill (an engineering practice not tied to one stack). It maintains a persistent, AI-readable knowledge layer in `.agents/context/`, derived from the actual project, so coding agents gain high-level understanding without repeatedly exploring the codebase. Use when starting work on a project, after significant project changes, or whenever the user references the project context.

## Scope

In scope:

- Behaviors described in `SKILL.md`.
- Bootstrapping and configuring the context system (update mode, detail level, scope, templates).
- Reading context before tasks; navigation via `README.md` / `structure.md`.
- Change-significance classification (Level 0–3) and mode-driven context updates.
- Context audits against the actual project.

Out of scope:

- Acting as a replacement for verifying the actual project. Source of truth is always source code, schema, configuration, infrastructure, and tests.
- Generating generic framework documentation unrelated to the specific project.
- Documenting every class/function/endpoint/column — the context must not duplicate the codebase.
- Trigger phrasings already covered by adjacent skills.

## Trigger Context

Common requests (should trigger):

- "set up project context for this repo"
- "initialize the project memory"
- "update the context"
- "audit the context"
- "save what we learned to the project context"
- Starting substantial work on a project where `.agents/context/` exists
- Completing changes to architecture, domains, APIs, data models, auth, or infrastructure

Should not trigger for:

- "refactor this function" (Level 0 internal change)
- "fix this typo" / "reformat this file"
- "write tests for the payments service"
- "explain how X works" answered directly from source code

## Source And Evidence Model

Authoritative sources:

- `SKILL.md` — runtime instructions and reference routing.
- `references/*.md` — bundled supplementary content (5 files).
- The maintained `.agents/context/` tree in target projects — derived data, never authoritative over the codebase.

Data that must not be stored in any context file:

- Secrets, credentials, tokens.
- Machine-specific filesystem paths (`/home/...`, `/Users/...`, drive letters with user names).
- Private URLs, customer data, or unredacted personal information.

### Coverage matrix

| Dimension | Status | Evidence |
|---|---|---|
| Runtime instructions | complete | `SKILL.md` |
| Reference architecture | complete | 5 files under `references/` |
| Bootstrap configuration model | complete | `references/bootstrap-and-config.md` |
| Change classification model | complete | `references/change-significance.md` |
| Update procedure + mode behavior | complete | `references/context-updates.md` |
| Audit procedure | complete | `references/context-audit.md` |
| Detail-level expectations | complete | `references/detail-levels.md` |

## Evaluation

Manual gates before committing changes to this skill:

- `SKILL.md` stays under ~1K tokens (2K hard cap); critical rules are front-loaded.
- Every reference file is linked from `SKILL.md` or another reachable file.
- No duplication between `SKILL.md` and `references/` beyond short summaries.
- All guidance is stack-neutral unless grounded in the target project.
- Content traces back to the original knowledge rules; no invented framework defaults.

Acceptance gates:

- A fresh agent given only `README.md` → `config.md` → `structure.md` can locate relevant context files for a task without exploring the whole directory.
- Level 0 examples produce no context writes; Level 3 examples produce updates per configured mode in every test scenario.

## Known Limitations

<!-- to fill in over time as drift surfaces: document recurring failure patterns,
     e.g. contexts that silently rot because update mode was set to manual and
     reports were ignored. -->

## Maintenance Notes

- Update `SKILL.md` when the workflow, iron rules, or routing change.
- Update this `SPEC.md` when intent, scope, triggers, or evidence model change.
- Grow `references/` rather than letting `SKILL.md` exceed its token cap.
- Keep trigger phrasings in sync between this spec and the frontmatter description.
