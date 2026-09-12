# Domain Layer: Models, Tools, Services

## Models

Add domain models using LocalMind's existing model infrastructure:

- Subclass the repository's base model (`DeclarativeBase` subclass).
- Register with the model registry decorator:
  `@register_model()` enables all CRUD ops;
  `@register_model(delete=False, update=False)` restricts them.
- Import the model where the database registers its tables so `init_db()`
  creates the table.

Keep `projects`/`tasks` as untouched example scaffolding. Chat/Message rows are
the conversation store used by the `Chat` service — never write them directly
from domain code.

## Generic CRUD Tools

LocalMind already generates generic CRUD tools from registered models. Use that
mechanism wherever a model is a standard CRUD entity — do not hand-write CRUD
tools the framework can generate safely. Adding a product feature means adding
a model, not a tool.

Only create custom tools when the operation contains domain logic that cannot
reasonably be expressed as generic CRUD.

## Domain Tools

Prefer meaningful, capability-oriented tools:

```text
Good:  search_products, get_stock, transfer_stock, receive_stock
Bad:   execute_sql, run_database_query, modify_any_table
```

For every tool ask: **Does the agent actually need this capability?**

Avoid tool explosion — a database model existing is never a reason for a tool
on its own.

## Domain Services

Complex business logic must not live inside tool classes. Prefer:

```text
Tool
 ↓
Domain Service
 ↓
SQLAlchemy / Models
```

Example:

```text
transfer_stock
    ↓
StockTransferService
    ↓
Stock + StockMovement
```

Tools should primarily: validate input, call the appropriate service, and
return a concise result.

## Memory

The generated application inherits LocalMind's memory capabilities
(chat context, global memory, chat history). Do not create a second unrelated
memory system. If the application requires domain-specific long-term memory,
extend the existing memory architecture rather than creating an unrelated
memory table.

## Implementation Conventions

Follow the codebase conventions from the cloned repository (2-space
indentation, type hints, `ENV.init()` before any import that reads env values
at import time). Run everything from the project root.