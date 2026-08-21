# Detail-Level Behavior

The detail level controls **how much knowledge is captured** — never whether captured information is accurate.

## Minimal

Prefer:

- Few files
- High-level architecture
- Critical conventions
- Major domains
- Critical workflows
- Critical integrations

Avoid:

- Implementation details
- Minor decisions
- Detailed domain internals

Document only information another AI agent critically needs to understand the project.

## Standard

Include:

- Architecture
- Conventions
- Database
- API
- Important workflows
- Important integrations
- Major domains
- Important decisions

Create only what is relevant to the actual project.

## Detailed

Everything from Standard plus:

- Complex domain relationships
- Important business rules
- Detailed workflows
- Architectural decisions
- Infrastructure
- Important implementation patterns
- Complex integrations
- Domain-specific architecture

Still never duplicate the source code. The objective is useful AI context, not a Markdown copy of the codebase.
