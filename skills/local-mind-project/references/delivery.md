# Documentation & Delivery

## Document the Project

Generate or update `README.md` with:

- project purpose,
- setup,
- configuration,
- database setup,
- model requirements,
- available tools,
- usage examples,
- scripts,
- testing.

Do not document inferred features that were not actually implemented.

## Project Manifest

Write a machine-readable manifest of the generated project. It gives the
generator (and LocalMind itself later) a description of what was created so the
project can be inspected, extended, migrated, or regenerated:

```yaml
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
```

The exact filename/location follows the conventions of the generated project
(e.g. `project.yaml` at the project root).

## Final Response to the User

After generation, report:

```text
Project generated successfully.

Project:   <name>
Base:      LocalMind
Models:    - ...
Services:  - ...
Tools:     - ...
Scripts:   - ...
Interfaces:- ...
LLM:       - ...
Tests:     - ...
Unverified:- ...
```

Also explain every architectural decision that was inferred rather than
explicitly requested, so the user knows exactly what the generator decided.