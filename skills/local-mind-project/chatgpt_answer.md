# Generate Project

## Purpose

Generate a complete application/project on top of the **LocalMind** framework.

This skill is responsible for taking a user's application idea and turning it into a working LocalMind-based project by:

1. cloning the LocalMind repository,
2. creating a new project workspace,
3. understanding the requested domain,
4. identifying required models,
5. identifying required tools,
6. identifying required services/business logic,
7. identifying required scripts/configuration,
8. asking the user to approve important generated components when they were not explicitly specified,
9. implementing the project,
10. testing it,
11. and leaving the generated project in a runnable state.

The generated project must remain compatible with the LocalMind architecture.

---

# Core Principle

LocalMind is the **agent framework**.

The generated application is a **domain implementation built on top of the framework**.

Do not modify the LocalMind core unnecessarily.

Think in terms of:

```text
LocalMind
│
├── Core framework
│   ├── Agent
│   ├── Chat
│   ├── Context
│   ├── Memory
│   ├── LLM providers
│   ├── Tool system
│   └── Database infrastructure
│
└── Generated application
    ├── Domain models
    ├── Domain tools
    ├── Services
    ├── Workflows
    ├── Scripts
    └── Configuration
```

The generated application must not turn LocalMind into a domain-specific framework.

---

# Repository

The base repository is:

```text
https://github.com/AbdoPrDZ/LocalMind
```

Before generating a project, clone the repository into the requested destination.

Do not assume that the currently checked-out directory is the destination project.

---

# First: Understand the User Request

Analyze the user's request and determine:

* application purpose,
* domain,
* main entities,
* required operations,
* workflows,
* expected users/actors,
* required integrations,
* required interfaces,
* persistence requirements,
* special business rules,
* expected scripts,
* expected model/provider requirements.

Do not immediately start coding.

First determine whether important architecture decisions are missing.

---

# Required Decisions

The agent should distinguish between:

## Explicit decisions

The user already specified them.

Example:

> "Create a warehouse application with Product, Warehouse and Stock models."

Do not ask the user to confirm those models again.

---

## Implied decisions

The user strongly implied them.

Example:

> "I need to track products in warehouses."

A `Product` and `Warehouse` model may be obvious, but relationships and additional models may still need confirmation if they materially affect the architecture.

---

## Generated decisions

The agent inferred or designed them.

Example:

```text
Product
Warehouse
Stock
StockMovement
```

If these were not specified by the user and materially affect the generated application, present them to the user for approval.

---

# User Agreement Before Generation

If important models, tools, workflows, or infrastructure are not specified, ask the user for agreement before creating them.

Do NOT ask about every tiny implementation detail.

Only ask about decisions that:

* affect the application's domain model,
* create persistent data,
* expose significant capabilities,
* affect security,
* affect external systems,
* affect architecture,
* or would be difficult to change later.

---

# Model Proposal

Before generating unspecified domain models, provide a concise proposal.

Example:

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

Allow the user to:

* approve all,
* reject all,
* modify them,
* approve selected models.

Do not continue with unapproved generated persistent models unless the user explicitly allows automatic generation.

---

# Tool Proposal

Tools must be treated separately from models.

For example:

```text
Models:
Product
Warehouse
Stock
StockMovement

Potential tools:

Product:
- create_product
- get_product
- search_products
- update_product
- delete_product

Warehouse:
- create_warehouse
- get_warehouse
- search_warehouses
- update_warehouse
- delete_warehouse

Stock:
- get_stock
- search_stock
- adjust_stock

Stock movement:
- create_stock_movement
- search_stock_movements
```

The agent should distinguish between:

### CRUD tools

Generated automatically from models where appropriate.

### Domain tools

Custom tools representing business operations.

Example:

```text
transfer_stock
reserve_stock
release_stock
receive_stock
```

Do not implement a business workflow as a collection of unrelated CRUD calls when a domain operation would be clearer.

---

# Tool Agreement

If the user did not specify the tools, propose them.

Example:

```text
I can create these tools for the application:

CRUD:
- product CRUD
- warehouse CRUD

Business tools:
- search_stock
- adjust_stock
- transfer_stock
- receive_stock

Should I create these tools?
```

The user may:

* approve all,
* select specific tools,
* modify tools,
* reject unnecessary tools.

---

# Avoid Tool Explosion

Do not create tools merely because a database model exists.

For every tool ask:

> Does the agent actually need this capability?

Prefer meaningful tools.

Bad:

```text
execute_sql
run_database_query
modify_any_table
```

Good:

```text
search_products
get_stock
transfer_stock
```

The LLM must never receive unrestricted SQLAlchemy or raw SQL access.

---

# Generated Application Structure

Use the existing LocalMind structure as the foundation.

Do not blindly duplicate the entire framework.

The generated project should conceptually resemble:

```text
GeneratedProject/
│
├── apps/
│
├── models/
│   ├── chat.py
│   ├── message.py
│   └── <domain models>
│
├── services/
│   ├── memory.py
│   └── <domain services>
│
├── tools/
│   ├── memory.py
│   ├── model.py
│   └── <domain tools>
│
├── utils/
│
├── resources/
│
├── scripts/
│   └── <project scripts>
│
├── tests/
│
├── main.py
├── database.py
├── requirements.txt
└── .env.example
```

Follow the actual repository structure rather than assuming this exact tree.

Always inspect the cloned repository before modifying it.

---

# Generic Tool Generation

LocalMind already supports generic CRUD tools based on model definitions.

Use that mechanism where appropriate.

Do not manually implement CRUD tools if the framework can generate them safely.

For example, if:

```text
Product
```

is a standard CRUD entity, use the generic model/tool system.

Only create custom tools when the operation contains domain logic that cannot reasonably be represented as generic CRUD.

---

# Domain Services

Complex business logic should not live directly inside tool classes.

Prefer:

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

Tools should primarily:

* validate input,
* call the appropriate service,
* return a concise result.

---

# Scripts

Determine whether the generated application needs additional scripts.

Possible examples:

```text
scripts/
├── seed_demo_data.py
├── reset_database.py
├── import_data.py
├── export_data.py
└── migrate_data.py
```

Do not create scripts just for the sake of creating them.

Create a script when it provides a useful operational or development capability.

If additional scripts are inferred and materially important, include them in the proposal before implementation.

---

# Configuration

Inspect the base repository's configuration system.

Do not create a second configuration mechanism unnecessarily.

Use the existing environment/configuration conventions.

If the generated project requires additional configuration, update:

```text
.env.example
```

and documentation.

Never commit secrets.

---

# Database

Use the existing SQLAlchemy/database infrastructure.

Domain models must be registered correctly with the project's database initialization/discovery mechanism.

If migrations are supported, create the required migration.

Do not replace the existing database architecture.

---

# LLM / Model Selection

Do not assume a particular LLM unless the user specifies one.

The generated project must remain compatible with LocalMind's provider abstraction.

If the application requires a specific model capability, explain why.

Examples:

```text
tool calling
vision
large context
multilingual
structured output
```

If the model is unspecified but model choice materially affects the project, ask the user.

Example:

```text
For this application I recommend Qwen3 4B because the project requires
tool calling and can run locally.

Should I use the default Qwen3 model?
```

Do not download large models without user agreement.

---

# External Integrations

If the requested application requires an external service, identify it before implementation.

Examples:

```text
PostgreSQL
Redis
Odoo
GitHub
Telegram
WhatsApp
REST API
SMTP
S3
```

Ask for agreement on the integration architecture when it is not already specified.

Never request or store secrets in source code.

Use environment variables.

---

# Interfaces

The current LocalMind architecture supports multiple frontends.

Do not automatically create:

```text
web
api
desktop
mobile
```

unless the user requests them.

If the user requests an interface, implement it through the shared `Chat` service rather than bypassing the core.

The interface should communicate with:

```text
Chat
```

not directly with:

```text
Agent
LLM
Database
```

---

# Project Generation Workflow

Use this workflow:

```text
1. Understand request
        ↓
2. Inspect LocalMind repository
        ↓
3. Identify domain
        ↓
4. Identify explicit requirements
        ↓
5. Identify inferred models
        ↓
6. Identify inferred tools
        ↓
7. Identify workflows/services
        ↓
8. Identify integrations/configuration
        ↓
9. Ask user for required decisions
        ↓
10. Clone LocalMind
        ↓
11. Create project structure
        ↓
12. Implement approved models
        ↓
13. Implement services
        ↓
14. Implement approved tools
        ↓
15. Add scripts/configuration
        ↓
16. Add tests
        ↓
17. Run tests
        ↓
18. Fix failures
        ↓
19. Update documentation
        ↓
20. Summarize generated project
```

---

# Clarification Rules

Do not overwhelm the user with a giant questionnaire.

Ask only the questions that materially affect implementation.

Prefer grouped questions.

Example:

```text
I can generate the application, but three architectural decisions are still open:

Models:
- Product
- Warehouse
- Stock

Tools:
- Product CRUD
- Stock lookup
- Stock transfer

Interface:
- CLI only
- CLI + API

I recommend:
- all three models
- all listed tools
- CLI + API

Do you agree with these defaults?
```

This is preferable to asking:

```text
What should Product be called?
What field should Product have?
What type should the ID be?
...
```

The agent should make reasonable low-level decisions itself.

---

# Default Agreement Strategy

If the user explicitly says:

> "Use your judgment."

or:

> "Generate whatever is required."

then the agent may automatically create inferred models, tools, services, and scripts.

However, it must still document the decisions afterward.

If the user says:

> "Ask me before creating anything."

then require approval for all significant generated components.

---

# Don't Ask Unnecessary Questions

Do not ask for:

* Python version if the repository already defines it
* database type if the project already has a default
* indentation style
* filenames
* internal class naming
* migration implementation
* obvious CRUD fields
* implementation details that can safely be inferred

Ask only about meaningful product/architecture decisions.

---

# Memory

The generated application inherits LocalMind's memory capabilities.

Do not create a second unrelated memory system.

The application should be able to use:

```text
Chat Context
Global Memory
Chat History
```

through LocalMind's existing mechanisms.

If the application requires domain-specific long-term memory, extend the existing memory architecture rather than creating an unrelated memory table.

---

# Security

Never generate unrestricted tools such as:

```text
execute_arbitrary_sql
execute_arbitrary_python
execute_shell
delete_everything
```

unless the user explicitly requests such capabilities and the security implications are clearly understood.

Domain tools should expose the minimum required capability.

Validate tool arguments using the existing tool/Pydantic mechanisms.

---

# Testing

Every generated application should have tests appropriate to its complexity.

At minimum test:

* model creation,
* important relationships,
* domain services,
* custom tools,
* critical business workflows.

Also verify that the inherited LocalMind functionality still works.

The generated application must not break:

```text
Chat
Message
Agent
Memory
LLM providers
Tool registry
```

---

# Documentation

Generate or update:

```text
README.md
```

with:

* project purpose,
* setup,
* configuration,
* database setup,
* model requirements,
* available tools,
* usage examples,
* scripts,
* testing.

Do not document inferred features that were not actually implemented.

---

# Final Verification

Before declaring the project complete:

1. Run the test suite.
2. Verify database initialization.
3. Verify the Agent starts.
4. Verify the selected LLM provider works.
5. Verify generated tools are registered.
6. Verify tool arguments validate correctly.
7. Verify at least one end-to-end domain workflow.
8. Verify existing LocalMind memory still works.
9. Verify no secrets were committed.
10. Verify documentation matches the actual implementation.

If something cannot be tested because an external dependency or API key is unavailable, explicitly report it.

Do not claim it works when it was not verified.

---

# Final Response to User

After generation, report:

```text
Project generated successfully.

Project:
<name>

Base:
LocalMind

Models:
- ...

Services:
- ...

Tools:
- ...

Scripts:
- ...

Interfaces:
- ...

LLM:
- ...

Tests:
- ...

Unverified:
- ...
```

Also explain any architectural decisions that were inferred rather than explicitly requested.

---

# Most Important Rules

1. **LocalMind remains generic.**
2. **Generated applications are domain layers on top of LocalMind.**
3. **Never blindly generate models/tools without considering whether they are actually needed.**
4. **Ask the user to approve significant inferred architecture when requirements are ambiguous.**
5. **Do not ask unnecessary low-level questions.**
6. **Use existing LocalMind abstractions before creating new ones.**
7. **Never give the LLM unrestricted database access.**
8. **Prefer domain services for complex business logic.**
9. **Do not introduce vector databases or unrelated infrastructure unless required.**
10. **Test the generated project before declaring it complete.**
11. **Do not modify the LocalMind core unless the requested application genuinely requires a framework-level change.**
12. **If a framework-level change is required, explain it before making the change when practical.**


----
One important addition I'd make

I'd also add a project manifest to generated projects eventually, something like:

project:
  name: inventory
  version: 0.1.0
  base: LocalMind

models:
  - Product
  - Warehouse
  - Stock

tools:
  - search_products
  - get_stock
  - transfer_stock

interfaces:
  - cmd

llm:
  provider: local
  model: qwen3-4b-instruct-gguf

That gives your generator a machine-readable description of what it created. Later, LocalMind could use the same manifest to inspect, extend, migrate, or regenerate an existing generated project.

Also, because your current LocalMind already has the generic CRUD tool generation and memory system documented in the repo, the skill should reuse those mechanisms rather than generating its own parallel systems.
