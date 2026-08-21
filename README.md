# ai-skills

Opinionated skills for AI coding agents. Each skill is a behavioral rule set that changes how an agent works — procedures, anti-patterns, and verification gates triggered by the task at hand. Skills add *discipline*, not knowledge: the model already knows how; the skill makes it actually do it.

## Layout

Each skill lives in `skills/<name>/`:

```text
skills/<name>/
├── SKILL.md        # runtime instructions: YAML frontmatter + behavioral rules
├── SPEC.md         # intent, scope, trigger context, evidence model, maintenance notes
├── references/     # supplementary content loaded on demand
├── scripts/        # optional helper scripts
└── assets/         # optional templates
```

Only the frontmatter `description` is loaded at startup; the body loads when the skill fires; references load on demand. Installing many skills costs near-zero tokens until one triggers.

## Skills

| Skill | Description |
|-------|-------------|
| [project-context](skills/project-context) | Maintains an AI-readable knowledge layer in `.agents/context/`: bootstrap on first contact, read before tasks, keep synchronized after significant changes (Level 0–3 classification), audit on request. Update modes: automatic / ask / manual. |

## Install

Copy a skill directory into your agent's skills location:

```bash
# user-level
~/.agents/skills/<skill-name>
# or per-project
.agents/skills/<skill-name>
```

Or point your agent's skill loader at this repository.

## Writing Skills

Conventions for skills in this repository:

- `SKILL.md` stays under ~1K tokens (2K hard cap); overflow goes into `references/`.
- Frontmatter `description` is keyword-rich and under 80 tokens — it is the only part always loaded, so it must contain the exact phrases users type.
- Critical rules come first (attention drops off); every "don't" pairs with a "do instead".
- Actions, not explanations — tell the agent what to do, skip what it already knows.
- One good default per decision; no menus of options.
