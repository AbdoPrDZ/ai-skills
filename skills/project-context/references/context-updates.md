# Context Updates

## Mode Behavior On Detected Changes

### automatic (Level 2–3)

1. Identify affected context.
2. Update it.
3. Verify consistency.
4. Continue/finish the task.

Do not ask the user for permission for every context update.

### ask

Ask before modifying context. The question must identify:

- What changed.
- Which context files are affected.
- Why the context needs updating.

Do not ask for insignificant changes.

### manual

Never silently modify context. Inform the user that the change affects project context:

```text
This change affects the project architecture.

The following context files may need updating:
- architecture.md
- domains/orders.md

Context update mode is currently manual.
No context files were modified.
```

If the user subsequently says "update the context", perform the synchronization.

## What Should Be Updated

Do not automatically rewrite the entire context after every significant change. Identify the smallest relevant context files:

```text
New payment workflow
    ↓
workflows.md, payments.md, possibly architecture.md
```

Do not rewrite unrelated files:

```text
database.md, frontend.md, deployment.md   ← only if actually affected
```

## Update Procedure

When a context update is required:

1. Identify what project knowledge changed.
2. Identify which context files describe that knowledge.
3. Read the relevant context files.
4. Verify the new behavior against the actual project.
5. Update only the affected context.
6. Check for contradictions with other context files.
7. Update related context if necessary.
8. Update `README.md` if navigation changed.
9. Update `structure.md` if the context organization changed.
10. Keep documentation concise and useful.

## Structure Changes

A structure change occurs when the organization of the context itself must change:

- A new major domain requires a dedicated context file.
- A context file becomes too large and must be split.
- Several related context files should be grouped into a domain.
- A new type of persistent project knowledge is introduced.
- A framework template requires additional context categories.

When the context structure changes:

1. Modify the context files.
2. Update `structure.md`.
3. Update `README.md` navigation if necessary.
4. Ensure the new structure follows the configured detail level.

## User Requests Override Normal Detection

If the user explicitly says "update the context" — update it regardless of the detected change level.

If the user says "do not update context" — do not update context for the current task unless required for safety or correctness of the task itself.

If the user asks to "audit the context" — perform a context audit per [context-audit.md](./context-audit.md).
