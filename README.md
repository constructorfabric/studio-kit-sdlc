# SDLC Kit

> This is a kit for the [**Constructor Studio**](https://github.com/constructorfabric/studio) project.

## Installation

```bash
# Install (--version is optional, defaults to latest)
cfs kit install constructorfabric/studio-kit-sdlc

# Update
cfs kit update sdlc

# Validate
cfs validate-kits .
```

## Overview

**Constructor Studio SDLC** is a kit that provides an artifact-first pipeline, turning intent into implementation through a fixed sequence of document layers with deterministic validation gates and end-to-end traceability.

- **Layered transformation**: PRD → ADR + DESIGN → DECOMPOSITION → FEATURE → CODE
- **Deterministic gates**: templates, IDs, cross-references, and task/acceptance criteria are validated by the skill engine at every step
- **Behavior spec**: the **FEATURE** layer expresses behavior as **Constructor Studio DSL (CDSL)** flows/algorithms/states plus definitions of done that can be implemented directly
- **Traceability chain**: each downstream artifact references upstream IDs; code keeps links via `@cpt-*` markers
- **Workflow orchestration**: artifact authoring and implementation presets delegate into shared Studio workflows for context discovery, brainstorm, planning, validation, review, and approved fix loops

## Pipeline Diagram

![**Constructor Studio** SDLC pipeline: PRD → ADR + DESIGN → DECOMPOSITION → FEATURE → CODE, with validation gates and ID traceability between layers](pipeline.drawio.svg)

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

**New to Constructor Studio SDLC?** Start here: **[QUICKSTART.md](guides/QUICKSTART.md)**

Learn Constructor Studio in 10 minutes with:
- **Exact prompts to copy-paste** into your AI chat
- **Complete example**: Task management API from start to finish
- **Common scenarios**: What to do when requirements change
- **Working with existing docs**: Use what you already have

## The SDLC Pipeline

| Artifact | Generation | Deterministic Validation | Feedback | Acceptance |
|----------|------------|--------------------------|----------|------------|
| **PRD** | Drafted from stakeholder input with required IDs | Template structure, ID format | Semantic review with findings browser and explicit fix approval | Product Managers & Architects alignment |
| **ADR** | Captures key architecture decisions with rationale | Template structure, ID format | Semantic review with findings browser and explicit fix approval | Architects alignment |
| **DESIGN** | Derived from PRD with architecture decisions | Cross reference ID and tasks validation | Semantic review vs PRD + ADR with findings browser and explicit fix approval | Architects alignment |
| **DECOMPOSITION** | Decomposed from DESIGN into implementable feature scope | Cross reference ID and tasks validation | Semantic review vs DESIGN with findings browser and explicit fix approval | Architects alignment |
| **FEATURE** | Expanded from DECOMPOSITION into **Constructor Studio DSL** (**CDSL**) flows/algorithms/states plus definitions of done | Cross reference ID and tasks validation | Semantic review vs DESIGN + DECOMPOSITION with findings browser and explicit fix approval | Architects & Developers alignment |
| **CODE** | Implemented from FEATURE with traceability in code comments | Cross reference ID and tasks validation | Semantic review vs FEATURE + DESIGN + DECOMPOSITION with findings browser and explicit fix approval | Developers & QA alignment |

## What the SDLC Kit Provides

The kit ships the following resources as ready-to-use files:

- **Structured Templates** (`template.md`) — templates for each artifact kind
- **Semantic Checklists** (`checklist.md`) — expert review criteria for quality gates
- **Examples** (`examples/`) — canonical examples for each artifact kind
- **Rules** (`rules.md`) — tasks and acceptance criteria for generation and validation
- **Constraints** (`constraints.toml`) — heading and ID constraints for deterministic validation
- **Workflows** (`workflows/`) — kit-specific workflows, including delegated artifact authoring/implementation presets plus standalone read-only PR review and PR status analysis routes
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
