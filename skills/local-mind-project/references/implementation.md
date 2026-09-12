# Scripts, Configuration, Database, LLM, Integrations, Interfaces

## Scripts

Create scripts only when they provide a useful operational or development
capability:

```text
scripts/
├── seed_demo_data.py
├── reset_database.py
├── import_data.py
├── export_data.py
└── migrate_data.py
```

Do not create scripts just for the sake of them. If materially important
inferred scripts are needed, include them in the proposal before
implementation.

## Configuration

Inspect the base repository's configuration system and reuse it. Do not create
a second configuration mechanism unnecessarily. If the generated project
requires additional configuration, update `.env.example` and documentation.
Never commit secrets — use environment variables.

## Database

Use the existing SQLAlchemy/database infrastructure. Domain models must be
registered correctly with the project's database initialization/discovery
mechanism. If migrations are supported, create the required migration. Do not
replace the existing database architecture.

## LLM / Model Selection

Do not assume a particular LLM unless the user specifies one. The generated
project must remain compatible with LocalMind's provider abstraction.

If the application needs a specific model capability, explain why:

```text
tool calling
vision
large context
multilingual
structured output
```

If the model is unspecified but model choice materially affects the project,
ask — recommending a default when clear. Example:

```text
For this application I recommend Qwen3 4B because the project requires
tool calling and can run locally.

Should I use the default Qwen3 model?
```

Do not download large models without user agreement.

## External Integrations

Identify required external services before implementation:

```text
PostgreSQL, Redis, Odoo, GitHub, Telegram, WhatsApp, REST API, SMTP, S3
```

Ask for agreement on the integration architecture when it is not already
specified. Never request or store secrets in source code — use environment
variables.

## Interfaces

Do not automatically create `web`, `api`, `desktop`, or `mobile` interfaces
unless the user requests them. If the user requests an interface, implement it
through the shared `Chat` service rather than bypassing the core:

```text
Interface  →  Chat  (not Agent / LLM / Database directly)
```

## Security

Never generate unrestricted tools such as:

```text
execute_arbitrary_sql
execute_arbitrary_python
execute_shell
delete_everything
```

unless the user explicitly requests such capabilities and understands the
security implications.

- Domain tools expose the minimum required capability.
- Validate tool arguments using the existing tool/Pydantic mechanisms.