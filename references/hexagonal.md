# Hexagonal (Ports & Adapters) Pattern

Best for: services requiring clean architecture, event-driven systems, DDD

## Template

```
project/
├── src/
│   ├── domain/                # Core: no framework deps, no infra deps
│   │   ├── model/             # Entities, value objects, aggregates
│   │   ├── ports/             # Interfaces (driving + driven ports)
│   │   │   ├── inbound/       # Use cases, input ports
│   │   │   └── outbound/      # Repository interfaces, output ports
│   │   └── services/          # Domain services
│   ├── adapters/              # Infrastructure: implements ports
│   │   ├── inbound/           # REST, GraphQL, CLI, gRPC, consumer
│   │   │   ├── rest/
│   │   │   ├── graphql/
│   │   │   └── cli/
│   │   └── outbound/          # DB, cache, HTTP, queue, filesystem
│   │       ├── postgres/
│   │       ├── redis/
│   │       └── http-client/
│   └── config/                # DI container, env, wiring
├── tests/
│   ├── unit/                  # Domain logic, pure
│   ├── integration/           # Adapter tests with real infra
│   └── e2e/                   # Full system tests
└── docs/
    ├── architecture.md        # Hexagonal diagram
    └── decisions/             # ADRs
```

## Rules

- `domain/` has zero imports from `adapters/`. Dependency rule: outer→inner.
- `ports/` defines interfaces; adapters implement them.
- Config wires ports to adapters (DI composition root).
- Domain model is pure: no serialization, no ORM annotations, no framework decorators.

## Anti-patterns

- ❌ `adapters/` importing `adapters/` (cross-adapter coupling)
- ❌ Domain objects with framework annotations (`@Entity`, `@Table`)
- ❌ Putting config inside domain/
- ❌ Leaking serialization logic into domain objects (use separate DTOs/mappers in adapters)
