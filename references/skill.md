# Skill Pattern

Best for: LLM skills (Claude Code, OpenCode, Codex, Cursor), plugins, agents, command collections

## Template

```
project/
├── SKILL.md            # Main entrypoint (required)
├── references/         # Documentation loaded on demand
├── scripts/            # Executable code (Python, Bash, JS)
├── examples/           # Usage examples
└── README.md           # Project documentation
```

## Rules

- `SKILL.md` is the only required file — the entrypoint the LLM loads when the skill triggers.
- `references/` holds detailed reference material, schemas, and patterns loaded on demand. Keep `SKILL.md` lean (<500 lines) and push detail here.
- `scripts/` is for executable code the skill orchestrates (Python validators, Bash formatters, JS generators).
- `examples/` is for runnable usage examples the LLM can reference.
- `README.md` is the GitHub-facing project documentation.
- Maximum 2 levels deep (`references/` can have files, not nested subdirectories).
- Metadata files like `CHANGELOG.md`, `LICENSE`, `.gitignore` belong at the repo level, not part of the skill structure.

## Anti-patterns

- ❌ Creating `src/` or `lib/` directories — `SKILL.md` IS the source.
- ❌ Splitting the skill across multiple `SKILL.md` files — use a plugin with `skills/` for that.
- ❌ Deep nesting beyond 2 levels — skills are simple by design.
- ❌ Including framework boilerplate (configs, build files, CI) inside the skill tree.
- ❌ Renaming `SKILL.md` — the LLM platform discovers it by exact filename.
