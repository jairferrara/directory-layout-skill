# 📐 directory-layout

> Skill para [OpenCode](https://opencode.ai) que genera estructuras de directorio óptimas para cualquier proyecto. Toma una idea, un archivo individual o una carpeta existente y produce layouts cohesivos basados en principios formales de organización de paquetes.

[![OpenCode](https://img.shields.io/badge/OpenCode-skill-6C47FF?style=flat-square)](https://opencode.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

---

## ✨ ¿Qué hace?

Dale una idea, un archivo, o una carpeta llena de archivos desordenados, y **directory-layout** te devuelve:

- Un árbol de directorio bien pensado con **READMEs** en cada nodo
- El **patrón óptimo** justificado con principios formales (CCP, CRP, REP, Screaming Architecture)
- Un **plan de migración** si estás refactorizando
- Con `--force`, ejecuta la migración real: mueve archivos, actualiza imports, genera READMEs

---

## 📦 Instalación

```bash
# 1. Clona este repo en la carpeta de skills de OpenCode
git clone https://github.com/jairferrara/directory-layout-skill.git \
  ~/.config/opencode/skills/directory-layout

# 2. (Opcional) Habilita la skill en tu opencode.json
#    Ya debería detectarse automáticamente al estar en la carpeta skills/
```

> **Requisito:** OpenCode v0.24+ con soporte para skills.

---

## 🚀 Uso rápido

```bash
/layout "un microservicio de pagos con FastAPI"
/layout src/main.py                       # generar layout a partir de un archivo
/layout ~/proyectos/caotico/              # analizar carpeta y proponer estructura
/layout . --dry-run                       # preview sin crear nada
/layout . --force                         # ejecutar la migración
/layout . --pattern hexagonal             # forzar un patrón específico
/layout . --explain                       # explicación detallada de cada decisión
/layout . --flat                          # estructura plana mínima
```

---

## 🧠 Patrones disponibles

| Patrón | Flag | Ideal para |
|--------|------|------------|
| **Layered** | `--pattern layered` | APIs, backends, servicios con capas técnicas claras |
| **Feature-based** | `--pattern feature` | Frontends, apps web, UIs con features distintas |
| **Domain-driven** | `--pattern domain` | Investigación, aprendizaje, proyectos multi-tópico |
| **Hexagonal** | `--pattern hexagonal` | Microservicios, arquitectura limpia, Ports & Adapters |
| **Monorepo** | `--pattern monorepo` | Proyectos multi-paquete, workspaces |
| **Flat-by-type** | `--pattern flat` | Librerías, CLIs, herramientas simples |

### Selección automática

Si no especificas `--pattern`, la skill detecta el patrón ideal según el contenido:

| Si detecta... | Usa... |
|---------------|--------|
| Flask, FastAPI, Express | `layered` |
| React, Vue, Svelte, `@Component` | `feature` |
| PyTorch, TensorFlow, NumPy | `domain` |
| `workspaces` en package.json / Cargo.toml | `monorepo` |
| Directorios `api/`, `domain/`, `infrastructure/` | `hexagonal` |
| Directorios por tema (física/, mates/) | `domain` |
| Directorios por feature (auth/, dashboard/) | `feature` |
| Archivos sueltos en la raíz | `flat-by-type` |

---

## 📋 Ejemplos

### Desde una idea

```
/layout "un CLI tool para convertir Markdown a LaTeX"
```

```
markdown-to-latex/
├── src/
│   ├── main.py           # entry point
│   ├── converters/       # md→tex conversion logic
│   ├── parsers/          # markdown AST parser
│   └── utils/
├── tests/
├── docs/
├── examples/
├── README.md
├── pyproject.toml
└── Makefile
```

Patrón: **flat-by-type** — los CLI tools no necesitan anidamiento profundo.

### Desde un archivo

Dado `app.py` con `from flask import Flask`:

```
/layout app.py
```

```
app/
├── api/
│   ├── routes.py
│   └── middleware.py
├── domain/
│   ├── models.py
│   └── services.py
├── infrastructure/
│   ├── database.py
│   └── redis.py
├── config/
│   └── settings.py
├── tests/
├── requirements.txt
└── README.md
```

### Refactorizando una carpeta existente

```
/layout ~/messy-project/ --explain
```

Analiza la estructura actual, computa el **cohesion score**, detecta el mejor patrón, y explica cada decisión sin modificar nada.

---

## ⚙️ Flags

| Flag | Descripción |
|------|-------------|
| `--dry-run` | Solo muestra el plan, no crea ni mueve nada |
| `--force` | Ejecuta la migración: crea directorios, mueve archivos, actualiza imports |
| `--pattern <name>` | Fuerza un patrón específico (layered, feature, domain, hexagonal, monorepo, flat) |
| `--flat` | Estructura plana mínima |
| `--explain` | Razona cada decisión estructural en detalle |

---

## ⚖️ Principios aplicados

| Principio | Nombre | Regla |
|-----------|--------|-------|
| **CCP** | Common Closure Principle | Lo que cambia por la misma razón, vive en el mismo directorio |
| **CRP** | Common Reuse Principle | Lo que se usa junto, vive en el mismo directorio |
| **REP** | Reuse/Release Equivalence | La unidad de reúso = la unidad de publicación |
| **Screaming Architecture** | — | La estructura de directorios grita qué es el proyecto |
| **Conway's Law** | — | La estructura refleja la comunicación del equipo |
| **ADP** | Acyclic Dependencies Principle | Sin ciclos en el grafo de dependencias |
| **SDP** | Stable Dependencies Principle | Depender en dirección a la estabilidad |

---

## 📁 Estructura del repo

```
directory-layout-skill/
├── README.md              ← esto
├── SKILL.md               ← skill principal
└── references/            ← plantillas de cada patrón
    ├── domain-driven.md
    ├── feature-based.md
    ├── flat-by-type.md
    ├── hexagonal.md
    ├── layered.md
    └── monorepo.md
```

---

## 🤝 Contribuir

¿Te falta un patrón? ¿Mejoras para un template existente? Abre un issue o PR en [github.com/jairferrara/directory-layout-skill](https://github.com/jairferrara/directory-layout-skill).

---

Hecho con ❤️ por [@jairferrara](https://github.com/jairferrara)
