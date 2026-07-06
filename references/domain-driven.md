# Domain-Driven Pattern

Best for: research, learning, multi-subject workspaces, data science

## Template

```
project/
├── <domain>/                  # Bounded context / subject area
│   ├── README.md              # Domain overview, goals, conventions
│   ├── ideas/                 # Semillas, concepts, proposals
│   │   ├── README.md
│   │   └── <idea>.md          # One file per idea with YAML frontmatter
│   ├── research/              # Active investigation projects
│   │   └── <project>/
│   ├── notes/                 # Study notes, reading notes, classes
│   │   └── <topic>.md
│   ├── experiments/           # Code, data, simulations
│   │   └── <experiment>/
│   ├── papers/                # Own publications (drafts, submissions)
│   │   └── <paper>/
│   └── references/            # External literature, bib, resources
├── <domain2>/                 # Another bounded context
│   └── ...                    # Same sub-structure
├── meta/                      # About the workspace itself
│   ├── README.md              # Architecture explanation
│   ├── templates/             # Reusable scaffolds
│   └── archive/               # Inactive but preserved material
└── README.md                  # Workspace overview
```

## Rules

- Each domain is a bounded context with its own vocabulary and conventions.
- `ideas/` is for *unstructured* thoughts. When they mature, they migrate to
  `research/` (active) or get written up as a paper.
- `archive/` preserves context. Never delete — archive with a note on why.
- Cross-domain connections go in `meta/` or as symlinks/docs referencing both.
- Every domain dir has a README explaining its scope and current state.

## Anti-patterns

- ❌ Files in root (every file belongs to a domain or meta/)
- ❌ Deep nesting (>4 levels). Prefer flat domain trees.
- ❌ Mixing domains (a physics concept in math/). If ambiguous, put it in both
  with a symlink or cross-reference.
- ❌ Archive as a graveyard. Archive entries should document *what was learned*.
