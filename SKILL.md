---
name: directory-layout
description: "Generates optimal directory layout patterns for any project. Takes an idea description, a single file, or an existing folder of files and produces a cohesive, principled directory structure. Applies formal package-organization principles (CCP, CRP, REP, Screaming Architecture) to produce structures that are predictable, cohesive, and future-proof."
---

# /layout

Generate a principled directory structure for any project — from scratch (given an idea) or by refactoring an existing folder into a coherent layout.

## Usage

```
/layout "una idea"                          # generate layout from a project idea
/layout path/to/file.py                      # generate layout based on a single file's domain
/layout path/to/folder/                      # refactor existing folder into optimal layout
/layout path/to/folder/ --dry-run            # preview without creating anything
/layout path/to/folder/ --force              # overwrite existing structure
/layout path/to/folder/ --pattern layered    # force a specific pattern
/layout path/to/folder/ --pattern domain     # domain-driven (default for research/learning)
/layout path/to/folder/ --pattern feature    # feature-based (default for web apps)
/layout path/to/folder/ --pattern hexagonal  # ports & adapters (default for services)
/layout path/to/folder/ --pattern monorepo   # multi-package monorepo
/layout path/to/folder/ --pattern skill      # LLM skill / plugin / agent
/layout path/to/folder/ --flat               # minimal flat structure
/layout path/to/folder/ --explain            # explain why this pattern was chosen
```

## What directory-layout is for

Every project needs a directory structure that *screams* what it is. This skill
applies formal software-architecture principles to generate one:

- **CCP (Common Closure Principle)** — things that change for the same reason stay together
- **CRP (Common Reuse Principle)** — things that are used together stay together
- **REP (Reuse/Release Equivalence Principle)** — things released together are one unit
- **Screaming Architecture** — the top-level structure reveals the project's purpose
- **Conway's Law** — structure mirrors communication patterns of the team/owner

It handles three input types:
1. **Idea** (text) → infer domain, generate scaffold from zero
2. **Single file** → detect language/framework, place it in appropriate context
3. **Existing folder** → analyze contents, regroup by cohesive concern, propose migration

## What You Must Do When Invoked

### Phase 0 — Understand the input

Determine what kind of input was given:

```
/layout "una idea"                     → type=idea, content="una idea"
/layout path/to/file.py                 → type=file, path=path/to/file.py
/layout path/to/folder/                 → type=folder, path=path/to/folder/
/layout path/to/folder/ --dry-run       → type=folder, flags=[dry-run]
```

If `--dry-run` is present: **never create files or directories**. Only output the
proposed tree as text and stop.

If `--explain` is present: output the proposed tree AND a detailed rationale for
every structural decision. See Phase 4.

If `--pattern` is present: skip pattern detection (Phase 2) and use the specified
pattern directly.

If no path and no idea text is given, use `.` (current directory) as a folder input.

If `--flat` is present: output the minimal flat structure for that project type.

### Phase 1 — Analyze

#### Subphase 1A: If input is an idea (string)

1. Infer the project domain from the description:
   - Look for keywords → map to project archetype
   - If ambiguous, ask one clarifying question (max 1)

2. Project archetypes and their default patterns:

| Keywords in idea | Archetype | Default pattern |
|---|---|---|
| web app, api, backend, server, rest, graphql | Web Service | layered |
| frontend, react, vue, svelte, ui, component | Frontend App | feature |
| mobile, ios, android, flutter, react-native | Mobile App | feature |
| data science, ml, notebook, analysis, experiment | Data Science | domain |
| research, paper, physics, biology, study | Research | domain |
| cli, command-line, tool, utility | CLI Tool | flat-by-type |
| library, package, sdk, client | Library | flat-by-type |
| game, unity, unreal, godot | Game | domain |
| infrastructure, terraform, k8s, docker | Infra | layered |
| monorepo, multi-package, workspaces | Monorepo | monorepo |
| service, microservice, event-driven | Service | hexagonal |
| learning, course, tutorial, study | Learning | domain |
| skill, SKILL.md, plugin, command, agent | LLM Skill | skill |
| *default / unknown* | General | layered |

2. Generate the base structure using the detected pattern.

#### Subphase 1B: If input is a single file

1. Read the file to detect:
   - Language (from extension or shebang)
   - Framework (from imports, dependencies)
   - Project type (infer from imports: flask → web, torch → ml, etc.)
   - The file's role in the implied project (entry point, module, config, etc.)

2. Generate a structure that contextualizes this file properly:
   - Place the file in its appropriate subdirectory (e.g. `src/`, `lib/`, etc.)
   - Add sibling directories for other concerns the file implies
3. If the file imports other local modules that don't exist, note them as
   "implied dependencies" in a `### Implied modules` section of the output.

#### Subphase 1C: If input is an existing folder

1. Scan the folder recursively (ignore common noise: `node_modules/`, `__pycache__/`,
   `.git/`, `venv/`, `.next/`, `dist/`, `build/`, `.DS_Store`)

2. Classify every file by:
   - **Role**: entry point, config, source, test, doc, data, asset, devops
   - **Layer**: presentation, business logic, data access, infrastructure
   - **Feature domain**: what business concept does it serve?
   - **Change frequency**: how often does it change? (estimate from git log if available)

3. Detect the project's implicit architecture by analyzing inter-file relationships:
   - Parse imports/requires to build a dependency graph
   - Identify clusters of highly-coupled files (candidates for co-location)
   - Identify boundary-crossing dependencies (candidates for interface extraction)

4. Compute a "cohesion score" for the current structure:
   - High cohesion = files that change together are close in the tree
   - Low cohesion = files that change together are scattered

5. Based on the analysis, recommend the optimal pattern. Show the cohesion score
   for the current structure vs. the proposed structure.

6. If `--force` was given, also **execute** the migration:
   - Create the new directory structure
   - Move files to their new locations
   - Update internal imports (relative paths, require/import statements)
   - Leave a `STRUCTURE_CHANGELOG.md` documenting every move

7. If `--force` was NOT given (default), output the proposed structure as a tree
   and list every file that would move, but do NOT create or move anything.

### Phase 2 — Pattern selection (auto-detect)

If the user did not specify `--pattern`, choose the best pattern:

**A. Layered** — for services with clear technical tiers

```
project/
├── api/              # HTTP handlers, routes, controllers
├── domain/           # Business logic, entities, use cases
├── infrastructure/   # DB, cache, external services
├── config/           # Configuration files
├── tests/
├── docs/
└── scripts/
```

**B. Feature-based** — for UIs and apps with distinct features

```
project/
├── features/
│   ├── auth/         # login, register, sessions
│   ├── dashboard/    # widgets, charts, overview
│   └── settings/     # preferences, profile
├── shared/           # common UI, utilities
├── config/
├── tests/
└── docs/
```

**C. Domain-driven** — for research, learning, multi-subject workspaces

```
project/
├── <domain>/
│   ├── README.md
│   ├── ideas/        # concepts, proposals
│   ├── notes/        # study notes, references
│   ├── research/     # active investigation
│   └── experiments/  # code, data, simulations
├── <domain2>/
└── meta/             # about the project itself
```

**D. Hexagonal (Ports & Adapters)** — for services needing clean architecture

```
project/
├── src/
│   ├── domain/       # Entities, value objects, domain services
│   │   ├── model/
│   │   ├── ports/    # Interfaces (driven & driving)
│   │   └── service/
│   ├── adapters/
│   │   ├── inbound/  # REST, GraphQL, CLI, events
│   │   └── outbound/ # DB, cache, HTTP clients
│   └── config/
├── tests/
│   ├── unit/
│   └── integration/
└── docs/
```

**E. Monorepo** — for multi-package projects

```
project/
├── packages/
│   ├── core/
│   ├── web/
│   ├── mobile/
│   └── shared/
├── tools/
├── configs/
├── docs/
├── package.json      # workspace root
└── tsconfig.json     # shared config
```

**F. Flat-by-type** — for libraries, CLIs, simple tools

```
project/
├── src/
│   ├── index.ts      # entry point
│   ├── commands/
│   ├── utils/
│   └── types/
├── tests/
├── docs/
├── examples/
└── scripts/
```

**G. Skill** — for LLM skills, plugins, agents

```
project/
├── SKILL.md            # Main entrypoint (required)
├── references/         # Documentation loaded on demand
├── scripts/            # Executable code (Python, Bash, JS)
├── examples/           # Usage examples
└── README.md           # Project documentation
```

**Selection rules** (apply in order):
1. If any file has `import flask` / `from fastapi` / `express` → **layered**
2. If the root directory contains a `SKILL.md` file → **skill**
3. If any file has `import React` / `from "react"` / `@Component` → **feature**
4. If any file has `import torch` / `import tensorflow` / `import numpy` → **domain**
5. If `package.json` has `"workspaces"` or root `Cargo.toml` has `[workspace]` → **monorepo**
6. If the folder has dirs like `api/`, `domain/`, `infrastructure/` already → **hexagonal**
7. If the folder has dirs named by subject (physics/, math/, biology/) → **domain**
8. If the folder has dirs named by feature (auth/, dashboard/, checkout/) → **feature**
9. If files are all in root with no structure → **flat-by-type** (most conservative)
10. If the cohesion score is <0.3 (highly scattered) → propose refactor with justification
11. If none match → **layered** (most universal)

### Phase 3 — Generate the structure

Build the directory tree using the selected pattern.

### Phase 3b — Migration plan (only for existing folders)

Generate a migration table:

```
CURRENT LOCATION                  → NEW LOCATION                     REASON
────────────────────────────────────────────────────────────────────────────
api/routes/users.py               → features/users/routes.py         feature co-location
models/user.py                    → features/users/domain.py         feature co-location
tests/test_user_api.py            → features/users/tests/            feature co-location
shared/utils.py                   → shared/utils.py                  (stays)
```

For each move, determine whether imports need updating:
- Python: `from models.user import User` → `from features.users.domain import User`
- Node: `require('../models/user')` → `require('../features/users/domain')`
- General: update any relative path that changes

### Phase 4 — Output

**Always output a tree view first:**

```
project/
├── features/
│   ├── auth/
│   │   ├── routes.py
│   │   ├── domain.py
│   │   └── tests/
│   │       └── test_auth.py
│   └── dashboard/
│       ├── routes.py
│       ├── domain.py
│       └── tests/
│           └── test_dashboard.py
├── shared/
│   └── utils.py
├── config/
│   └── settings.py
└── tests/
    └── test_integration.py
```

Then append:
- **Pattern used**: "feature-based"
- **Rationale**: 1-2 sentences on why this pattern fits
- **Principles applied**: which of CCP, CRP, REP, Screaming Architecture motivated each choice

If `--explain` was given, add a detailed section per directory explaining:
- Why this directory exists
- What belongs here (and what doesn't)
- How it relates to sibling directories
- The cohesion argument for its placement

If `--force` was given and files were actually moved, also output:
```
✓ Created 12 directories
✓ Moved 34 files
✓ Updated 28 import paths
→ See STRUCTURE_CHANGELOG.md for full migration log
```

### Phase 5 — Logging (only when files were moved)

If `--force` was used, write a `.layout-meta.json` in the root as a log of what was done:

```json
{
  "pattern": "feature",
  "generated": "2026-07-06T...",
  "domains": ["auth", "dashboard", "settings"],
  "principles": ["CCP", "CRP", "Screaming Architecture"],
  "migration_log": "STRUCTURE_CHANGELOG.md"
}
```

This is the only file created beyond directories and moved files.

## Design principles reference

These are the formal software-engineering principles behind every layout decision.
Reference them in your rationale to justify each choice.

### Package Cohesion Principles (Robert C. Martin)

| Acronym | Name | Rule | Directory equivalent |
|---|---|---|---|
| **REP** | Reuse/Release Equiv. | Granule of reuse = granule of release | A directory should be shipable/versionable independently |
| **CCP** | Common Closure | Classes that change together belong together | Files modified for the same reason should share a parent dir |
| **CRP** | Common Reuse | Classes that are used together belong together | Files that are always imported together should share a parent dir |

### Package Coupling Principles

| Acronym | Name | Rule |
|---|---|---|
| **ADP** | Acyclic Dep. | No cycles in the directory dependency graph |
| **SDP** | Stable Dep. | Depend in direction of stability |
| **SAP** | Stable Abstractions | Stable dirs should be abstract; concrete dirs should be volatile |

### Architectural Principles

- **Screaming Architecture** (R. C. Martin): The top-level directory listing
  should tell a reader what the project *is*, not what framework it uses
- **Conway's Law**: Directory structure mirrors communication structure of
  the organization that produces it
- **Package-by-feature > Package-by-layer**: Feature packages are more cohesive
  and more independently modifiable than layer packages
- **Stable Dependencies Principle**: A package should depend only on packages
  that are more stable than it is

## Reference: Pattern templates

Each pattern has a canonical template stored as a reference. When generating,
load the template and specialize it for the project:

- `references/layered.md`
- `references/feature-based.md`
- `references/domain-driven.md`
- `references/hexagonal.md`
- `references/monorepo.md`
- `references/flat-by-type.md`
- `references/skill.md`

Create these on first use if they don't exist. Each reference contains:
- Full directory tree for the pattern
- Rules for what goes where
- Common anti-patterns (what *not* to put in each directory)

## Cohesion scoring

When analyzing an existing folder, compute a simple cohesion metric:

```
cohesion = Σ(mutual_dependencies_within_dir) / Σ(total_dependencies)
```

- 1.0 = perfect (every dependency is within the same directory)
- 0.0 = worst (every dependency crosses directory boundaries)
- >0.7 = acceptable
- <0.4 = needs refactoring

Use import/require/include statements to build the dependency graph. In mixed
languages or when imports can't be parsed, estimate by file-pair co-change
frequency (from git log: files committed together → likely coupled).

## Examples

### From idea

```
/layout "a CLI tool for converting markdown to LaTeX"
```

Output:
```
markdown-to-latex/
├── src/
│   ├── converters/       # md→tex conversion logic
│   ├── parsers/          # markdown AST parser
│   └── utils/
├── tests/
├── docs/
├── examples/
└── scripts/
```

Pattern: **flat-by-type** — CLI tools don't need deep nesting.
Principles: Screaming Architecture (src/ says "this is code"),
CCP (converters change together, parsers change together).

### From single file

Given `app.py` containing `from flask import Flask`:

```
/layout app.py
```

Detects: Flask web app. Places the file and creates the directory scaffold:

```
app/
├── api/
│   └── routes.py          ← app.py moved here
├── domain/
├── infrastructure/
├── config/
└── tests/
```

File placement: `app.py` → `api/routes.py` (it's a Flask app with routes).

### From existing folder

```
/layout ~/messy-project/
```

Analyzes, detects a web app with scattered files, computes cohesion=0.25,
recommends feature-based layout, and produces a migration plan for every file.
Without `--force`, does nothing — just shows the plan.

---

## Honesty Rules

- Never propose a structure deeper than 4 levels without justification
- Never delete files or directories without `--force`
- Never create new files (only directories, moved files, and log files are created)
- Never modify file contents without `--force`
- If `--dry-run`, output the plan and absolutely nothing else
- Always mention what pattern was chosen and why
- If cohesion is already >0.7, say so and ask if the user still wants restructuring
