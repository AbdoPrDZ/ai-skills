# Context Audit

An audit compares the maintained context with the actual project.

## Procedure

1. Read `README.md`.
2. Read `config.md`.
3. Read `structure.md`.
4. Inspect the relevant project architecture.
5. Identify outdated context.
6. Identify missing important context.
7. Identify contradictory context.
8. Identify unnecessary/outdated files.
9. Report findings.
10. Update according to the configured update mode or explicit user request.

## Scope Rule

An audit should not attempt to inspect every source file unless necessary. Focus on the architecture and knowledge areas described by the existing context.

## After The Audit

- Outdated → correct it (the actual project is authoritative).
- Missing → add only what the configured detail level and scope justify.
- Contradictory → resolve in favor of the actual project, then fix the losing source of the contradiction.
- Unnecessary files → propose removal; follow the update mode before deleting.
