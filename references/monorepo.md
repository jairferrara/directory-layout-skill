# Monorepo Pattern

Best for: multi-package projects, shared codebases, platform teams

## Template

```
project/
├── packages/
│   ├── core/                  # Shared logic, no framework deps
│   │   ├── src/
│   │   ├── tests/
│   │   └── package.json
│   ├── web/                   # Frontend app
│   │   ├── src/
│   │   ├── public/
│   │   ├── tests/
│   │   └── package.json
│   ├── api/                   # Backend service
│   │   ├── src/
│   │   ├── tests/
│   │   └── package.json
│   └── shared/                # Shared UI components or types
│       ├── src/
│       └── package.json
├── tools/                     # Build scripts, codegen, lint rules
│   └── eslint-config/
├── configs/                   # Shared configs
│   ├── tsconfig.base.json
│   └── jest.config.base.js
├── docs/                      # Root docs
├── .github/                   # CI/CD
├── package.json               # Workspace root
├── pnpm-workspace.yaml        # or lerna.json, nx.json
├── tsconfig.json
└── README.md
```

## Rules

- Root package.json is workspace config only — no app code.
- Each package has its own `package.json`, `tsconfig.json`, tests.
- `tools/` → scripts used by CI or dev workflow, not by the apps.
- Cross-package imports go through the package's public API (index.ts), not internal paths.
- One version rule: shared dependencies at root, conflicting deps per package.

## Anti-patterns

- ❌ App code in root (package.json should be workspace metadata only)
- ❌ Direct imports of internal files across packages (use package exports)
- ❌ One package depending on another's devDependencies
- ❌ Ignoring build order (use tools like nx, turborepo, or lerna for dependency graph)
