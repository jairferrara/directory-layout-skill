# Flat-by-Type Pattern

Best for: CLI tools, libraries, SDKs, simple scripts, single-module projects

## Template

```
project/
├── src/
│   ├── main.py                # Entry point
│   ├── commands/              # CLI commands
│   ├── utils/                 # Pure utility functions
│   └── types/                 # Type definitions / schemas
├── tests/
│   ├── test_commands/
│   └── test_utils/
├── docs/
│   ├── usage.md
│   └── api.md
├── examples/
│   └── basic.py
├── scripts/                   # Build, release, dev helpers
├── README.md
└── pyproject.toml             # or Cargo.toml, package.json
```

## Rules

- One level of grouping inside `src/` is enough. If you need more, consider a
  different pattern (layered or feature).
- `src/` is the only source directory. No `lib/`, `app/`, `core/` variants.
- `tests/` mirrors `src/` structure: `tests/commands/` → `src/commands/`.
- `examples/` is for usage examples, not tests.
- `scripts/` is for release automation, not for application features.

## Anti-patterns

- ❌ Deep nesting. If `src/commands/subcommands/parser/tokens/` exists, you've
  outgrown this pattern.
- ❌ Business logic in entry point (`main.py` should be a thin CLI wrapper).
- ❌ Mixing examples with tests (examples should be runnable, not assertable).
