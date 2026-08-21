# Bootstrap And Configuration

## When To Bootstrap

If no context exists and the user asks for coding work: do not require the user to manually explain the context system. Bootstrap it according to the skill.

Determine before writing files:

- Project type, framework, main technologies.
- Architecture.
- Major domains.
- Important integrations.
- Existing conventions.

If the user has not configured update mode, detail level, and scope, ask for these preferences during bootstrap, then persist them in `.agents/context/config.md`.

## Step 1 — Check For Context

Check whether `.agents/context/` exists:

- Exists → read `README.md`, `config.md`, `structure.md`; follow the configured rules.
- Does not exist → bootstrap.

Never recreate existing context merely because it does not match a preferred default structure. If existing files are incomplete, improve them only when necessary.

## Ask During Bootstrap

### Update Mode

Ask which behavior the user wants when significant project changes are detected:

| Mode | Behavior |
|---|---|
| `automatic` | Automatically update context when a significant project change is detected. |
| `ask` | Detect significant changes and ask the user before modifying context. |
| `manual` | Never modify automatically. Inform the user that context may need updating; update only on explicit request. |

The user may choose different terminology; persist the selected behavior in `config.md`.

### Detail Level

Ask how detailed the context should be. The detail level controls **how much knowledge is captured**, never whether captured information is accurate:

- `minimal` — document only what another AI agent critically needs to understand the project.
- `standard` — document important architecture, domains, workflows, conventions, integrations, and relationships.
- `detailed` — additionally capture substantial architectural knowledge, important implementation patterns, business rules, workflows, relationships, decisions, and other information useful for complex project work.

See [detail-levels.md](./detail-levels.md) for full content expectations per level.

### Scope

Recommended options:

- `project` — only project-wide knowledge.
- `project + domains` — project-wide knowledge plus important business/technical domains.
- `deep` — additionally include important implementation-level knowledge.

Do not document every source file merely because `deep` is selected. The objective remains useful AI context, not generating a duplicate codebase in Markdown.

## Config File

Example `config.md`:

```text
# Context Configuration

Update Mode: automatic

Detail Level: standard

Scope: project + domains

Template: laravel
```

## Framework / Project Template

Detect the project technology and architecture. Examples:

- Laravel
- React
- Flutter
- Odoo
- Django
- Node.js
- Generic/custom project

Rules:

- If a framework-specific `structure.md` template is already installed, follow it.
- If no template exists, create a sensible generic structure.
- Do not invent framework-specific rules that are not supported by the actual project.

Framework-specific knowledge must come from:

1. An installed context template.
2. The actual project.
3. Reliable project configuration/code.

Do not assume a project follows framework defaults. For example, a Laravel project may use a non-standard architecture — document the actual architecture rather than the framework's conventional architecture.

Templates are allowed to define recommended files/directories, minimal/standard/detailed structures, file responsibilities, domain organization, and framework-specific context categories (they populate `structure.md`). A template must not blindly overwrite existing project knowledge: if a context already exists, preserve existing information and merge carefully.

## Domain Context

For projects with multiple domains:

```text
.agents/context/
└── domains/
    ├── README.md
    ├── authentication.md
    ├── orders.md
    ├── payments.md
    └── notifications.md
```

Only create domain context for meaningful domains. A domain file normally contains:

- Purpose.
- Important entities.
- Business rules.
- Relationships.
- Workflows.
- Important integrations.
- Important constraints.
- Project-specific implementation knowledge.

The exact contents are controlled by `structure.md`.
