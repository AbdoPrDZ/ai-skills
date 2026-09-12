# Testing & Final Verification

## Testing

The level of testing is agreed during approvals
([approvals.md](./approvals.md)):

- **none** — no tests are generated, but the final verification checklist
  below still applies where feasible.
- **minimal** — smoke tests: model creation, tool registration, one end-to-end
  domain workflow.
- **full** (default recommendation) — the minimum set below.

At the agreed level, tests cover:

- model creation,
- important relationships,
- domain services,
- custom tools,
- critical business workflows.

Also verify that the inherited LocalMind functionality still works — the
generated application must not break `Chat`, `Message`, `Agent`, `Memory`,
LLM providers, or the tool registry.

## Project Generation Workflow To This Point

```text
1. Understand request
2. Inspect LocalMind repository
3. Identify domain
4. Identify explicit requirements
5. Identify inferred models
6. Identify inferred tools
7. Identify workflows/services
8. Identify integrations/configuration
9. Ask user for required decisions
10. Clone LocalMind
11. Create project structure
12. Implement approved models
13. Implement services
14. Implement approved tools
15. Add scripts/configuration
16. Add tests
17. Run tests
18. Fix failures
```

## Final Verification

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

If something cannot be tested because an external dependency or API key is
unavailable, explicitly report it. Do not claim it works when it was not
verified.