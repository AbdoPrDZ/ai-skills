# Repository & Generated Structure

## Clone

Before generating, clone the base repository into the requested destination:

```text
https://github.com/AbdoPrDZ/LocalMind
```

Do not assume that the currently checked-out directory is the destination
project. Clone a fresh copy if in doubt.

## Inspect Before Modifying

Always inspect the cloned repository before modifying it. Follow its actual
structure rather than assuming any tree — the generated project uses LocalMind
as its foundation but does not blindly duplicate the framework.

## Generated Application Conceptually

```text
GeneratedProject/
│
├── apps/                        # interfaces (cmd, web, api, desktop...)
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

The actual repository may differ — inspect it and adapt.

## Boundaries

The generated project is a **domain layer on top of LocalMind**. Reuse the
framework's components (registry, generic CRUD, `Chat`, memory, provider
abstraction) instead of creating parallel systems. Only touch the framework
itself when the requested application genuinely requires a framework-level
change — and in that case, explain the change before making it when practical.