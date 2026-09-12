# User Agreement Before Generation

Ask for agreement only on decisions that:

- affect the application's domain model,
- create persistent data,
- expose significant capabilities,
- affect security,
- affect external systems,
- affect architecture,
- or would be difficult to change later.

Do NOT ask about every tiny implementation detail.

## Model Proposal

Before generating unspecified domain models, present a concise proposal:

```text
I identified these domain models:

1. Product
   - name
   - sku
   - price

2. Warehouse
   - name
   - location

3. Stock
   - product
   - warehouse
   - quantity

4. StockMovement
   - product
   - warehouse
   - quantity
   - movement_type

These models are inferred from your requirements.
Do you want me to create them?
```

Let the user approve all, reject all, modify them, or approve selected models.
Do not continue with unapproved generated persistent models unless the user
explicitly allows automatic generation.

## Tool Proposal

Treat tools separately from models. Models and tools are decided
independently. Present the distinct constructs:

```text
Models:
Product, Warehouse, Stock, StockMovement

Potential tools:

Product:        create_product, get_product, search_products,
                update_product, delete_product
Warehouse:      create_warehouse, get_warehouse, search_warehouses,
                update_warehouse, delete_warehouse
Stock:          get_stock, search_stock, adjust_stock
Stock movement: create_stock_movement, search_stock_movements
```

Two kinds of tools:

- **CRUD tools** — generated automatically from models where appropriate.
- **Domain tools** — custom tools for business operations
  (`transfer_stock`, `reserve_stock`, `release_stock`, `receive_stock`).

Do not implement a business workflow as a collection of unrelated CRUD calls
when a domain operation would be clearer.

The user may approve all, select specific tools, modify them, or reject
unnecessary ones.

## Testing Parameter

Ask the user whether they want tests generated, and at what level, as part of
the grouped project decisions:

| Level | Scope |
|---|---|
| none | No tests generated. |
| minimal | Smoke tests only: model creation, tool registration, one end-to-end workflow. |
| full (recommended) | Models, relationships, domain services, custom tools, critical workflows, plus verification that inherited LocalMind functionality (Chat, Agent, Memory, tool registry) still works. |

Example:

```text
Testing: none | minimal | full

I recommend: full.

Do you agree?
```

If the user does not answer, default to `full` for non-trivial projects and
`minimal` for trivial ones; never silently generate `none`. The agreed level
is honored in [verification.md](./verification.md) and reported in the final
summary ([delivery.md](./delivery.md)).

## Default Agreement Strategy

| User stance | Generator behavior |
|---|---|
| "Use your judgment" / "Generate whatever is required" | Automatically create inferred models, tools, services, scripts, and tests at the recommended level — but document the decisions afterward. |
| "Ask me before creating anything" | Require approval for all significant generated components. |

## Don't Ask Unnecessary Questions

Do not ask about Python version, database type, indentation, filenames,
internal class naming, migration implementation, or obvious CRUD fields.
Prefer grouped questions:

```text
I can generate the application, but four architectural decisions are open:

Models:    Product, Warehouse, Stock
Tools:     Product CRUD, Stock lookup, Stock transfer
Interface: CLI only | CLI + API
Testing:   none | minimal | full

I recommend: all three models, the listed tools, CLI + API, full tests.
Do you agree with these defaults?
```

This is far better than asking "What should Product be called?", "What type
should the ID be?", and so on — the agent makes reasonable low-level decisions
itself.