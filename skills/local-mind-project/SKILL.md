---
name: local-mind-project
class: task
description: >-
  Generate a complete application/project on top of the LocalMind agent
  framework: clone the LocalMind repository, then build a domain layer
  (models, tools, services, scripts, configuration, interfaces) over its shared
  core — never rewriting the framework. Use when the user asks to create, build,
  or generate a project/app/product based on LocalMind, or to scaffold a
  LocalMind-powered application from an idea or requirements.
---

# LocalMind Project Generation

Turn an application idea into a runnable LocalMind-based project. The agent
clones LocalMind, understands the requested domain, designs the domain layer,
gets user approval for significant inferred decisions, implements, tests, and
delivers a working project.

## Core Principle

```text
LocalMind             →  the agent framework
                        (Agent, Chat, Context, Memory, LLM providers,
                         Tool system, Database). Keep it generic.

Generated application →  a domain layer on top of the framework
                        (domain models, tools, services, workflows,
                         scripts, configuration).
```

Do not modify the LocalMind core for a generated app unless it genuinely
requires a framework-level change — and if it does, explain it before making it.

## Repository

Base repository: `https://github.com/AbdoPrDZ/LocalMind`. **Clone** it into the
requested destination before generating. Never assume the currently
checked-out directory is the destination project.

## Workflow

1. Understand the request, classify decisions → [understand.md](./references/understand.md)
2. Inspect the cloned LocalMind repository
3. Propose and get approval for significant inferred choices → [approvals.md](./references/approvals.md)
4. Clone LocalMind, plan the generated structure → [structure.md](./references/structure.md)
5. Implement models, domain services, tools → [domain.md](./references/domain.md)
6. Add scripts, configuration, integrations, interfaces → [implementation.md](./references/implementation.md)
7. Test and verify end-to-end → [verification.md](./references/verification.md)
8. Document, write the project manifest, report → [delivery.md](./references/delivery.md)

## Non-Negotiable Rules

1. **LocalMind remains generic.** Generated apps are a domain layer on it.
2. **Never generate tools/models blindly** — every one must be justified by a real need.
3. **Ask for approval** on significant inferred architecture when requirements are ambiguous.
4. **Never ask unnecessary low-level questions** — make reasonable low-level decisions yourself.
5. **Reuse LocalMind abstractions first** (model registry, generic CRUD, `Chat`, Memory) before creating parallels.
6. **Never give the LLM unrestricted access** — no raw SQL/python/shell execution tools.
7. **Domain logic lives in services**, not in tool classes.
8. **Interfaces reach the LLM only through the `Chat` service.**
9. **Test the generated project before declaring it complete.**
10. **Never claim it works when it was not verified.**

## Anti-Patterns

| Anti-pattern | Do instead |
|---|---|
| Copying/forking the framework into the generated app | Build a domain layer that imports the unchanged core |
| A tool per model column or table | One meaningful CRUD set + domain tools that express business operations |
| Bypassing `Chat` in a new interface | Call `chat.send(...)` from the interface |
| Implementing business workflows as piles of CRUD calls | A domain service + a domain tool for the operation |
| Introducing a second memory/vector system | Extend LocalMind's existing memory architecture |