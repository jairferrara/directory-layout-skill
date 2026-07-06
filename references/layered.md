# Layered Pattern

Best for: web services, backends, APIs

## Template

```
project/
├── api/                # HTTP layer: routes, controllers, middleware, serializers
│   ├── routes/
│   ├── middleware/
│   └── serializers/
├── domain/             # Business logic: entities, services, use cases
│   ├── entities/
│   ├── services/
│   └── ports/          # Interfaces for infrastructure adapters
├── infrastructure/     # External integrations: DB, cache, queue, HTTP clients
│   ├── database/
│   ├── cache/
│   └── clients/
├── config/             # Configuration, env vars, settings
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── docs/
├── scripts/            # Dev scripts, migrations, seeders
├── README.md
└── <build-tool-files>
```

## Rules

- `api/` → only request/response handling. No business logic.
- `domain/` → zero dependencies on framework or infrastructure.
- `infrastructure/` → implements interfaces defined in `domain/ports/`.
- `config/` → never imported by tests (tests override config).
- `tests/` → mirrors source structure: `tests/api/`, `tests/domain/`, etc.

## Anti-patterns

- ❌ Business logic in route handlers
- ❌ Framework imports in domain code
- ❌ Circular deps: domain → infrastructure → domain
- ❌ Putting config in domain/
