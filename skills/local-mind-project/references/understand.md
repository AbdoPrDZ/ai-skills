# Understand the Request

Analyze the user's request and determine:

- application purpose and domain,
- main entities,
- required operations and workflows,
- expected users/actors,
- required integrations,
- required interfaces,
- persistence requirements,
- special business rules,
- expected scripts,
- required model/provider capabilities.

Do not immediately start coding during this step.

## Decision Taxonomy

Classify every design element you may need:

| Type | Meaning | Example | Action |
|---|---|---|---|
| Explicit | The user specified it | "Create a warehouse app with Product, Warehouse, Stock models." | Do not re-ask; implement it. |
| Implied | Strongly implied by the request | "I need to track products in warehouses." | `Product`/`Warehouse` are obvious; confirm only what materially changes architecture. |
| Generated | Inferred or designed by the agent | `Stock`, `StockMovement` | Propose for approval ([approvals.md](./approvals.md)). |

## Clarification Rules

- Do not overwhelm the user with a giant questionnaire.
- Ask only questions that materially affect implementation; group them.
- Make reasonable low-level decisions yourself:
  - Python version — the repository already defines it.
  - Database type — the project already has a default.
  - Indentation, filenames, internal class naming, migration mechanics.
  - Obvious CRUD fields.

Ask only about meaningful product/architecture decisions.