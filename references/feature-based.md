# Feature-Based Pattern

Best for: frontend apps, CRUD applications, mobile apps

## Template

```
project/
├── features/
│   ├── <feature>/
│   │   ├── components/    # UI components
│   │   ├── hooks/         # State/logic hooks
│   │   ├── api/           # Feature-specific API calls
│   │   ├── types/         # Feature-specific types
│   │   ├── utils/         # Feature-specific utilities
│   │   └── tests/
│   └── <feature2>/
├── shared/                # Shared code across features
│   ├── components/
│   ├── hooks/
│   ├── utils/
│   └── types/
├── config/
├── tests/                 # Integration/e2e tests
├── public/                # Static assets (frontend)
├── docs/
├── README.md
└── <config-files>
```

## Rules

- One directory per feature. If a feature has >10 files, split into sub-features.
- `shared/` → only code used by 2+ features. Extract here, don't add preemptively.
- Each feature owns its tests: `features/auth/tests/` not `tests/features/auth/`.
- Feature directories should be independently removable (low coupling).

## Anti-patterns

- ❌ Shared code that only one feature uses
- ❌ Features importing from sibling features (extract to shared/ instead)
- ❌ Putting routing/config in feature dirs (belongs in config/ or root)
- ❌ Deeply nested features (>3 levels deep)
