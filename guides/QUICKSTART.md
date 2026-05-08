# Cyber Constructor SDLC Quick Start

**Learn Cyber Constructor SDLC in 10 minutes with real prompts and examples**

Cyber Constructor SDLC works through the `cf-constructor` skill — enable it with `cf-constructor on` and use natural language prompts prefixed with `cf-constructor`. The skill handles artifact discovery, template loading, validation, and traceability automatically.

---

## What You'll Learn

1. **Exact prompts to type** — copy-paste into your AI chat
2. **Complete pipeline** — from requirements to validated code
3. **Reverse engineering** — create artifacts from existing code
4. **Working with existing docs** — import what you already have

---

## The Pipeline

Cyber Constructor SDLC = **Design First, Code Second**

```mermaid
flowchart LR
    PRD["**PRD**<br/>300+ criteria"]
    ADR["**ADR**<br/>270+ criteria"]
    DESIGN["**DESIGN**<br/>380+ criteria"]
    DECOMP["**DECOMPOSITION**<br/>130+ criteria"]
    FEATURE["**FEATURE**<br/>380+ criteria"]
    CODE["**CODE**<br/>200+ criteria"]

    PRD --> ADR --> DESIGN --> DECOMP --> FEATURE --> CODE
```

| Artifact | Purpose |
|----------|---------|
| **PRD** | Product requirements — actors, capabilities, requirements, use cases, constraints |
| **ADR** | Architecture Decision Record — context, options, decision, consequences |
| **DESIGN** | Technical architecture — components, interfaces, data models, sequences |
| **DECOMPOSITION** | Feature breakdown — ordered implementation units with dependencies |
| **FEATURE** | Feature acceptance criteria, flows, algorithms, edge cases, definitions of done |
| **CODE** | Implementation — tagged with `@cpt-*` markers for traceability |

**Key principle**: If code contradicts design, fix design first, then regenerate code.

Learn what each artifact means: [TAXONOMY.md](TAXONOMY.md)

---

## Getting Started

| Prompt | What happens |
|--------|--------------|
| `cf-constructor on` | Enables Cyber Constructor mode, discovers config, loads project context |
| `cf-constructor init` | Creates `cf-constructor/` directory with `.core/`, `.gen/`, `config/` and initializes `artifacts.toml` |
| `cf-constructor show pipeline` | Displays current artifact hierarchy and validation status |

**Customizing artifact locations:**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor set artifacts_dir to docs/design/` | Changes default base directory for new artifacts |
| `cf-constructor register PRD at docs/requirements/product-requirements.md` | Registers existing file as PRD artifact |
| `cf-constructor move PRD to docs/requirements/PRD.md` | Moves artifact and updates registry |
| `cf-constructor show artifact locations` | Displays current paths from `artifacts.toml` |

---

## Example Prompts — Greenfield Project

**PRD — Product Requirements**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor make PRD` | Generates PRD interactively, asking for context |
| `cf-constructor make PRD for task management API` | Generates PRD with actors, requirements, flows |
| `cf-constructor draft PRD from README` | Creates PRD draft from existing README |
| `cf-constructor extend PRD with notifications` | Adds new capability to existing PRD |
| `cf-constructor validate PRD` | Full validation (300+ criteria) |
| `cf-constructor validate PRD semantic` | Semantic validation only (completeness, clarity) |
| `cf-constructor validate PRD structural` | Structural validation only (format, IDs, template) |
| `cf-constructor validate PRD refs` | Check cross-references to other artifacts |

**ADR — Architecture Decision Records**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor make ADR for PostgreSQL` | Creates ADR for technology choice |
| `cf-constructor make ADR for REST vs GraphQL` | Creates ADR comparing approaches |
| `cf-constructor draft ADR from discussion` | Extracts decision from conversation |
| `cf-constructor list ADRs` | Shows all ADRs with status |
| `cf-constructor validate ADR` | Validates all ADRs (270+ criteria each) |
| `cf-constructor validate ADR 0001` | Validates specific ADR by number |
| `cf-constructor validate ADR semantic` | Semantic validation (rationale quality) |
| `cf-constructor validate ADR structural` | Structural validation (format, sections) |

**DESIGN — Technical Architecture**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor make DESIGN` | Creates DESIGN from PRD interactively |
| `cf-constructor make DESIGN from PRD` | Transforms PRD into architecture |
| `cf-constructor extend DESIGN with caching layer` | Adds new component to existing DESIGN |
| `cf-constructor update DESIGN components` | Updates component section |
| `cf-constructor validate DESIGN` | Full validation (380+ criteria) |
| `cf-constructor validate DESIGN semantic` | Semantic validation (consistency, completeness) |
| `cf-constructor validate DESIGN structural` | Structural validation (format, IDs) |
| `cf-constructor validate DESIGN refs` | Check references to PRD/ADR |

**DECOMPOSITION — Feature Breakdown**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor decompose` | Creates DECOMPOSITION interactively |
| `cf-constructor decompose into features` | Creates ordered feature list with dependencies |
| `cf-constructor decompose by capability` | Groups features by business capability |
| `cf-constructor decompose by layer` | Groups features by technical layer |
| `cf-constructor add feature to decomposition` | Adds new feature entry |
| `cf-constructor validate DECOMPOSITION` | Full validation (130+ criteria) |
| `cf-constructor validate DECOMPOSITION semantic` | Semantic validation (coverage, dependencies) |
| `cf-constructor validate DECOMPOSITION structural` | Structural validation (format, IDs) |

**FEATURE — Feature Specification**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor make FEATURE for auth` | Creates feature design for auth |
| `cf-constructor make FEATURE for task-crud` | Creates detailed feature design |
| `cf-constructor draft FEATURE from code` | Reverse-engineers feature from implementation |
| `cf-constructor extend FEATURE auth with MFA` | Adds scenario to existing feature |
| `cf-constructor validate FEATURE auth` | Full validation (380+ criteria) |
| `cf-constructor validate FEATURE auth semantic` | Semantic validation (flows, edge cases) |
| `cf-constructor validate FEATURE auth structural` | Structural validation (CDSL format, IDs) |
| `cf-constructor validate FEATURE auth refs` | Check references to DESIGN/DECOMPOSITION |

**CODE — Implementation**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor implement {slug}` | Generates code from FEATURE |
| `cf-constructor implement {slug} step by step` | Implements with user confirmation |
| `cf-constructor implement {slug} tests first` | Generates tests first, then code |
| `cf-constructor implement {slug} flow {flow-id}` | Implements specific flow only |
| `cf-constructor implement {slug} api` | Implements API layer only |
| `cf-constructor implement {slug} tests` | Generates tests only |
| `cf-constructor continue implementing {slug}` | Continues partial implementation |
| `cf-constructor implement {slug} remaining` | Implements only unimplemented parts |
| `cf-constructor sync code with FEATURE {slug}` | Updates code to match FEATURE |
| `cf-constructor add markers to {path}` | Adds markers to existing code |
| `cf-constructor add markers for {slug}` | Adds markers matching FEATURE |

**CODE — Validation**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor validate code` | Validates all code markers |
| `cf-constructor validate code for {slug}` | Validates specific feature |
| `cf-constructor validate code coverage` | Reports implementation coverage % |
| `cf-constructor validate code coverage for {slug}` | Coverage for specific feature |
| `cf-constructor validate code orphans` | Finds orphaned markers |
| `cf-constructor validate code refs` | Validates marker references |
| `cf-constructor validate code markers` | Checks marker format |
| `cf-constructor list code markers` | Lists all markers |
| `cf-constructor list code markers for {slug}` | Lists markers for feature |
| `cf-constructor show uncovered flows` | Lists flows without code |
| `cf-constructor compare code to FEATURE {slug}` | Shows drift from feature |
| `cf-constructor find missing implementations` | Lists unimplemented elements |

**Pipeline & Cross-Artifact**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor validate all` | Validates entire hierarchy |
| `cf-constructor validate refs` | Checks all cross-references |
| `cf-constructor show pipeline` | Displays artifact status |
| `cf-constructor coverage report` | Summary of implementation coverage |

---

## Example Prompts — Brownfield Project

**Reverse-engineer from code**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor reverse PRD from codebase` | Extracts requirements from existing implementation |
| `cf-constructor reverse DESIGN from src/` | Documents current architecture from code |
| `cf-constructor reverse FEATURE from src/auth/` | Creates FEATURE from auth module |
| `cf-constructor reverse FEATURE auth from code` | Same, using feature slug |
| `cf-constructor analyze src/api/` | Systematic code analysis with checklist |

**Import from existing docs**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor make PRD from docs/requirements.txt` | Extracts requirements into PRD format |
| `cf-constructor make PRD from README` | Creates PRD from project README |
| `cf-constructor make PRD from this conversation` | Creates PRD from stakeholder discussion |
| `cf-constructor import OpenAPI as DESIGN` | Converts API spec into DESIGN |
| `cf-constructor import db-schema.sql as DESIGN data model` | Extracts data model from SQL |
| `cf-constructor convert user-stories.md to PRD` | Transforms user stories to PRD |

**Sync and compare**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor compare DESIGN to code` | Shows drift between design and implementation |
| `cf-constructor sync DESIGN from code` | Updates DESIGN to match current code |
| `cf-constructor diff FEATURE auth` | Shows changes since last validation |

---

## Example Prompts — Evolution & Maintenance

**Extend artifacts**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor extend PRD with payments` | Adds capability to PRD |
| `cf-constructor extend DESIGN with caching` | Adds component to DESIGN |
| `cf-constructor extend FEATURE auth with MFA` | Adds scenario to feature |
| `cf-constructor add feature billing to decomposition` | Adds feature entry |

**Update and propagate**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor update PRD requirements` | Updates requirements section |
| `cf-constructor propagate PRD changes to DESIGN` | Updates DESIGN from PRD changes |
| `cf-constructor sync DESIGN from code` | Updates DESIGN to match implementation |

**Impact analysis**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor show impact of removing FR-AUTH-002` | Traces downstream references |
| `cf-constructor show impact of changing component auth` | Shows affected artifacts |
| `cf-constructor deprecate feature user-import` | Marks deprecated across artifacts |

**Refactoring**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor refactor component auth-service` | Updates DESIGN and FEATUREs |
| `cf-constructor rename feature billing to payments` | Renames across all artifacts |
| `cf-constructor split feature payments` | Creates sub-features |
| `cf-constructor merge features billing and invoicing` | Combines features |

---

## Example Prompts — Review & Quality

**Validation modes**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor validate {ARTIFACT}` | Full validation (structural + semantic) |
| `cf-constructor validate {ARTIFACT} semantic` | Semantic only (content quality, completeness) |
| `cf-constructor validate {ARTIFACT} structural` | Structural only (format, IDs, template compliance) |
| `cf-constructor validate {ARTIFACT} refs` | Cross-reference validation only |
| `cf-constructor validate {ARTIFACT} quick` | Fast check (critical issues only) |
| `cf-constructor validate all` | Full validation of entire pipeline |
| `cf-constructor validate all semantic` | Semantic validation across all artifacts |

**Traceability queries**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor trace FR-AUTH-001` | Shows path: PRD → DESIGN → FEATURE → CODE |
| `cf-constructor trace feature auth` | Shows all references to auth feature |
| `cf-constructor find orphans` | Lists IDs with no upstream/downstream refs |
| `cf-constructor find orphan code` | Code markers referencing non-existent IDs |
| `cf-constructor find orphan requirements` | Requirements not covered by DESIGN |
| `cf-constructor list unimplemented` | Features without code coverage |
| `cf-constructor coverage report` | Implementation coverage by artifact |
| `cf-constructor show refs for DESIGN` | All references in DESIGN artifact |

**Review prompts**

| Prompt | What the agent does |
|--------|---------------------|
| `cf-constructor review PRD` | Deep review with 300+ criteria |
| `cf-constructor review DESIGN for consistency` | Checks for internal contradictions |
| `cf-constructor review FEATURE auth for completeness` | Validates edge cases and error handling |
| `cf-constructor review PR #42 against FEATURE` | Checks PR implements feature items |
| `cf-constructor review code for auth` | Reviews implementation quality |

---

## Live Example

[Taskman (Cyber Constructor example project)](https://github.com/cyberfabric/cyber-constructor-examples-taskman) — a complete task management project with the full Cyber Constructor artifact set and implementation.

---

## Guides by Scenario

| Scenario | Guide | Key Point |
|----------|-------|-----------|
| **Greenfield** | [GREENFIELD.md](GREENFIELD.md) | Start from PRD, work down to code |
| **Brownfield** | [BROWNFIELD.md](BROWNFIELD.md) | Start anywhere — code-only, bottom-up, or full |
| **Modular monolith** | [MONOLITH.md](MONOLITH.md) | Project-level + module-level artifacts |

**Brownfield flexibility**: No required order. Start with code-only (checklist benefits), add FEATURE designs for complex areas, add DESIGN later. Or go full top-down. Your choice.

## Reference

- Artifact taxonomy: [TAXONOMY.md](TAXONOMY.md)
- Kit overview: [README.md](../README.md)
