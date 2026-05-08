# SDLC Kit

> This is a kit for the [**Cyber Constructor**](https://github.com/cyberfabric/cyber-constructor) project.

## Installation

```bash
# Install (--version is optional, defaults to latest)
cfc kit install cyberfabric/cyber-constructor-kit-sdlc

# Update
cfc kit update sdlc

# Validate
cfc validate-kits .
```

## Overview

**Cyber Constructor SDLC** is a kit that provides an artifact-first pipeline, turning intent into implementation through a fixed sequence of document layers with deterministic validation gates and end-to-end traceability.

- **Layered transformation**: PRD → ADR + DESIGN → DECOMPOSITION → FEATURE → CODE
- **Deterministic gates**: templates, IDs, cross-references, and task/acceptance criteria are validated by the skill engine at every step
- **Behavior spec**: the **FEATURE** layer expresses behavior as **Cyber Constructor DSL (CDSL)** flows/algorithms/states plus definitions of done that can be implemented directly
- **Traceability chain**: each downstream artifact references upstream IDs; code keeps links via `@cpt-*` markers

## Pipeline Diagram

![**Cyber Constructor** SDLC pipeline: PRD → ADR + DESIGN → DECOMPOSITION → FEATURE → CODE, with validation gates and ID traceability between layers](pipeline.drawio.svg)

Each layer **transforms** the previous artifact into a new form while **preserving traceability through IDs and references**:

| From | To | Transformation |
|------|-----|----------------|
| **PRD** | ADR + DESIGN | WHAT → HOW (architecture decisions and design) |
| **DESIGN** | DECOMPOSITION | Architecture → decomposition to features |
| **DECOMPOSITION** | FEATURE | Features → detailed behavior + definitions of done |
| **FEATURE** | CODE | Detailed behavior + DoD → implementation, source code |

The LLM reads the upstream artifact, understands its intent, and generates a downstream artifact of a **different kind** with explicit ID references back to the source. This creates a **traceable chain** from requirements to implementation.

---
## Quick Start

**New to Cyber Constructor SDLC?** Start here: **[QUICKSTART.md](guides/QUICKSTART.md)**

Learn Cyber Constructor in 10 minutes with:
- **Exact prompts to copy-paste** into your AI chat
- **Complete example**: Task management API from start to finish
- **Common scenarios**: What to do when requirements change
- **Working with existing docs**: Use what you already have

## The SDLC Pipeline

| Artifact | Generation | Deterministic Validation | Feedback | Acceptance |
|----------|------------|--------------------------|----------|------------|
| **PRD** | Drafted from stakeholder input with required IDs | Template structure, ID format | Semantic review vs industry best practices | Product Managers & Architects alignment |
| **ADR** | Captures key architecture decisions with rationale | Template structure, ID format | Semantic review vs industry best practices | Architects alignment |
| **DESIGN** | Derived from PRD with architecture decisions | Cross reference ID and tasks validation | Semantic review vs PRD + ADR + industry best practices | Architects alignment |
| **DECOMPOSITION** | Decomposed from DESIGN into implementable feature scope | Cross reference ID and tasks validation | Semantic review vs DESIGN + industry best practices | Architects alignment |
| **FEATURE** | Expanded from DECOMPOSITION into **Cyber Constructor DSL** (**CDSL**) flows/algorithms/states plus definitions of done | Cross reference ID and tasks validation | Semantic review vs DESIGN + DECOMPOSITION + industry best practices | Architects & Developers alignment |
| **CODE** | Implemented from FEATURE with traceability in code comments | Cross reference ID and tasks validation | Semantic review vs FEATURE + DESIGN + DECOMPOSITION + industry best practices | Developers & QA alignment |

## What the SDLC Kit Provides

The kit ships the following resources as ready-to-use files:

- **Structured Templates** (`template.md`) — templates for each artifact kind
- **Semantic Checklists** (`checklist.md`) — expert review criteria for quality gates
- **Examples** (`examples/`) — canonical examples for each artifact kind
- **Rules** (`rules.md`) — tasks and acceptance criteria for generation and validation
- **Constraints** (`constraints.toml`) — heading and ID constraints for deterministic validation
- **Workflows** (`workflows/`) — kit-specific workflows (PR review, PR status)
- **Skill Extensions** — kit-specific skill content injected into `.gen/SKILL.md`

## References

- [Code Quality Checklist](codebase/checklist.md) — code review criteria

## Documentation

**Quick Start**:
- [QUICKSTART.md](guides/QUICKSTART.md) — 10-minute guide with real prompts and examples

**Implementation Guides**:
- [GREENFIELD.md](guides/GREENFIELD.md) — starting new projects from scratch
- [BROWNFIELD.md](guides/BROWNFIELD.md) — integrating with existing codebases (start anywhere)
- [MONOLITH.md](guides/MONOLITH.md) — working with modular monoliths
- [TAXONOMY.md](guides/TAXONOMY.md) — artifact taxonomy and terminology
