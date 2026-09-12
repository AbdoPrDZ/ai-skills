# local-mind-project Specification

## Intent

`local-mind-project` is a `task`-class skill. It turns an application idea
into a runnable project built on top of the LocalMind agent framework: it
clones the LocalMind repository and composes a domain layer (models, tools,
services, scripts, configuration, interfaces) over the shared core, keeping
the framework generic.

This document is the design specification. Operational instructions live in
`SKILL.md` and `references/`; they are not duplicated here.

## Why This Skill Exists

LocalMind's value is that one generic core — Agent, Chat, Context, Memory,
LLM providers, generic CRUD tool generation, database — powers many domain
applications behind interchangeable interfaces. Building a new application by
hand usually means copying the core, hardcoding tools per model, or bypassing
the `Chat` service, which destroys that value. This skill encodes the
discipline of building **on top of** LocalMind so generated projects stay
compatible, upgradable, and regenerable.

## Philosophy

> Generate the smallest useful domain layer on top of an unchanged LocalMind
> core.

Consequences:

- The framework is the platform; the generated code is the product.
- Reuse framework abstractions (model registry, generic CRUD, `Chat`, memory)
  before creating parallel systems.
- Ambiguity in requirements is the generator's trigger to **propose**, not to
  silently guess.
- Approval is scoped to significant, hard-to-reverse decisions only; low-level
  decisions are made by the generator.
- A project is done only after it is tested and honestly verified.

## Source Of Truth

Generated projects and this skill both derive from the actual LocalMind repo:

```text
Actual LocalMind code > this skill's expectations > assumed conventions
```

When the repository differs from this skill's expectations, follow the
repository, and correct the skill when warranted (see Maintenance Notes).
The source-of-truth priority holds for generated projects too: the generated
code must match what was actually implemented and verified.

## Lifecycle

```text
Understand → Decide → Approve → Structure → Implement → Verify → Deliver
```

1. **Understand** — analyze the request; classify explicit / implied /
   generated needs.
2. **Decide** — make reasonable low-level decisions; identify the significant
   open decisions.
3. **Approve** — get user agreement on significant generated
   models/tools/integrations/interfaces.
4. **Structure** — clone LocalMind into the destination; inspect it; plan the
   domain layer.
5. **Implement** — models, services, tools, scripts, configuration, interfaces.
6. **Verify** — tests plus the end-to-end verification checklist.
7. **Deliver** — documentation, project manifest, honest final report.

## Approval Model

Two approval defaults, chosen by the user's stance:

| User stance | Generator behavior |
|---|---|
| "Use your judgment" / "Generate whatever is required" | Auto-generate inferred components; document the decisions afterward. |
| "Ask me before creating anything" | Require approval for every significant generated component. |

Regardless of stance, never surprise the user with persistent models, exposed
capabilities, security-affecting choices, or external-system integrations that
were never mentioned.

## Tool-Safety Invariants

- No unrestricted SQL / Python / shell execution tools for the LLM.
- Tools expose the minimum required capability for the domain.
- Tool arguments are validated by the existing tool/Pydantic mechanisms.
- Interfaces reach the LLM only through the `Chat` service.
- Domain logic lives in services; tools validate, call, and respond.

## Evidence Model

Authoritative sources:

- `SKILL.md` — runtime instructions and routing.
- `references/*.md` — detailed procedures (request analysis, approvals,
  structure, domain layer, implementation, verification, delivery).
- The actual LocalMind repository — authoritative over this skill's
  expectations.

Data that must never enter a generated project's committed files:

- Secrets, credentials, tokens, API keys.
- Anything unverified presented as working.

## Evaluation Gates

Before shipping changes to this skill:

- `SKILL.md` ≤ ~1K tokens preferred, 2K hard cap; critical rules front-loaded.
- Procedures referenced, not duplicated, across `SKILL.md` and `references/`.
- All internal links resolve.
- Rule set internally consistent with the approval model and tool-safety
  invariants.
- A fresh agent following the references can produce a runnable, tested
  project without guessing at significant decisions.

## Future Extensibility

- The project manifest (`delivery.md`) as a machine-readable contract LocalMind
  itself can consume to inspect, extend, migrate, or regenerate existing
  generated projects.
- Interface generators (web/api/desktop) that always go through the `Chat`
  service.
- Domain template packs (payments, auth, inventory) as composable fragments for
  the generated domain layer.

## Known Limitations

<!-- Fill over time as gaps surface: e.g. regenerating an already-modified
     generated project, monorepo destination edge cases, or interfaces that
     cannot bypass the shared core even when the user asks. -->

## Maintenance Notes

- Update `SPEC.md` when intent, philosophy, lifecycle, or extensibility change
  — not for operational tweaks.
- Update `SKILL.md` when runtime behavior or routing changes.
- Grow `references/` rather than letting `SKILL.md` exceed its token cap.
- Keep trigger phrasings synchronized between this spec and the frontmatter
  description.