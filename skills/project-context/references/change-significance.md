# Change Significance

After completing a meaningful coding task, evaluate whether the changes affect the project's persistent knowledge. Classify using the levels below.

## Level 0 — No Context Update

Implementation details, not persistent project knowledge:

- Variable renaming.
- Formatting.
- Typographical fixes.
- Minor CSS changes.
- Small internal refactors.
- Simple bug fixes that do not alter architecture or behavior.
- Tests that do not introduce new project behavior.
- Internal implementation changes that do not affect project understanding.

## Level 1 — Evaluate

Consider whether context needs updating. Only update if the change materially affects how another AI should understand or work with the project:

- New helper abstractions.
- New reusable components.
- Minor dependency changes.
- Small internal architectural improvements.
- New implementation patterns.
- Changes to existing conventions.

## Level 2 — Context Update Recommended

Context should normally be updated for:

- New modules.
- New major components.
- New APIs.
- New database entities.
- New external integrations.
- New business workflows.
- New authentication/authorization mechanisms.
- New queues or asynchronous processing systems.
- Significant changes to existing domains.
- Significant dependency changes.
- New infrastructure components.

## Level 3 — Mandatory Context Update

Changes affecting fundamental project knowledge:

- Architecture changes.
- Major database architecture changes.
- Authentication architecture changes.
- Authorization architecture changes.
- Major business-rule changes.
- Major workflow/state-machine changes.
- Deployment architecture changes.
- Major framework or technology changes.
- Introduction/removal of a major system.
- Major external integration changes.
- Major domain restructuring.

Mode handling for detected changes (including Level 2–3) is defined in [context-updates.md](./context-updates.md):

- `automatic` → update relevant context before finishing.
- `ask` → ask the user before making the context change.
- `manual` → report that context may require updating; do not modify silently.
