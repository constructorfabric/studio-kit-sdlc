# Brownfield Guide

Use this guide when you already have a codebase and want to adopt Cyber Constructor.

All prompts work through the `cf-constructor` skill — enable it with `cf-constructor on` and use natural language prompts.

## Goal

Adopt Cyber Constructor incrementally — start with what makes sense for your project, not a fixed sequence.

## Key Principle: Start Anywhere

Unlike greenfield projects, **brownfield has no required order**. You can:

- Start with **any artifact** — PRD, DESIGN, FEATURE, or even just CODE
- Work **top-down** (PRD → DESIGN → CODE) or **bottom-up** (CODE → FEATURE → DESIGN)
- Adopt **incrementally** — use only what you need, add more later
- Use **code-only mode** — just Cyber Constructor's code generation with checklist benefits

**Even with zero artifacts**, Cyber Constructor's code generation uses the `code-checklist` internally for quality guidance.

---

## Recommended First Step: Auto-Configuration

Before choosing an adoption scenario, run auto-config to let Cyber Constructor learn your project:

| Prompt | What happens |
|--------|--------------|
| `cf-constructor auto-config` | Scans project and generates rules, registry entries, and AGENTS.md integration |

**What it does** (6 phases):
1. **Project Scan** — reads directory structure, entry points, language/framework detection
2. **Documentation Discovery** — finds existing docs, READMEs, API specs, schemas
3. **System Detection** — identifies system boundaries, subsystems, components
4. **Rule Generation** — creates per-system convention rules in `{cf-constructor-path}/config/rules/{slug}.md`
5. **AGENTS.md Integration** — adds WHEN rules to `{cf-constructor-path}/config/AGENTS.md` so the agent loads the right rules at the right time
6. **Registry Update** — registers detected systems and codebase paths in `{cf-constructor-path}/config/artifacts.toml`

**When to run**:
- After `cf-constructor init` on an existing project
- After major structural changes (new modules, framework migration)
- When Cyber Constructor doesn't know your conventions yet

**What you get**: The agent will now understand your project structure, naming conventions, tech stack, and test patterns — making all subsequent prompts (from any adoption scenario below) more accurate.

> **Note**: Auto-config is also triggered automatically by the `generate` workflow when it detects a brownfield project with no rules.

---

## Adoption Scenarios

### Scenario A: Code-Only

You just want better code generation. No artifacts needed.

| Prompt | What happens |
|--------|--------------|
| `cf-constructor implement feature auth` | Generates code using code-checklist quality guidance |
| `cf-constructor add markers to src/auth/` | Adds traceability markers to existing code |
| `cf-constructor validate code` | Validates code quality and marker correctness |

**Benefits**: Quality-guided code generation, consistent patterns, code traceability.

### Scenario B: Feature-First (Bottom-Up)

You want to document existing features, then build up.

```
1. cf-constructor reverse FEATURE from src/auth/     → Creates FEATURE from code
2. cf-constructor reverse FEATURE from src/billing/  → Creates another FEATURE from code
3. cf-constructor decompose from features            → Creates DECOMPOSITION from FEATUREs
4. cf-constructor make DESIGN from DECOMPOSITION     → Creates DESIGN from structure
5. cf-constructor make PRD from DESIGN               → (optional) Creates PRD from DESIGN
```

**When to use**: You want to document what exists without changing code.

### Scenario C: Design-First (Middle-Out)

You want to capture architecture, then decompose into features.

```
1. cf-constructor reverse DESIGN from codebase       → Extracts architecture from code
2. cf-constructor decompose                          → Creates feature breakdown
3. cf-constructor make FEATURE for {slug}            → Creates detailed features
4. cf-constructor implement {slug}                   → Implements with traceability markers
```

**When to use**: You want architectural control before feature work.


### Scenario D: Full Top-Down

You want complete documentation from requirements to code.

```
1. cf-constructor reverse PRD from codebase          → Extracts requirements
2. cf-constructor make DESIGN from PRD               → Creates architecture
3. cf-constructor decompose                          → Creates feature breakdown
4. cf-constructor make FEATURE for {slug}            → Creates detailed features
5. cf-constructor implement {slug}                   → Implements with traceability markers
```

**When to use**: New team members, compliance requirements, or major refactoring.

### Scenario E: Gradual Adoption

Start minimal, add artifacts as needed.

```
Week 1: cf-constructor implement {slug}              → Code-only, with checklist
Week 2: cf-constructor make FEATURE for {slug}       → Add features for complex work
Week 3: cf-constructor decompose                     → Organize features
Later:  cf-constructor make DESIGN                   → Document architecture
```

**When to use**: You want low-friction adoption with growing benefits.

---

## What You Will Produce

Cyber Constructor artifacts registered in `{cf-constructor-path}/config/artifacts.toml` ([taxonomy](TAXONOMY.md)):

| Artifact | Default Location |
|----------|------------------|
| PRD | `architecture/PRD.md` |
| ADR | `architecture/ADR/*.md` |
| DESIGN | `architecture/DESIGN.md` |
| DECOMPOSITION | `architecture/DECOMPOSITION.md` |
| FEATURE | `architecture/features/{slug}.md` |

## How to Provide Context

Brownfield work is context-heavy. Add context to control what the agent reads and how it maps existing reality into Cyber Constructor artifacts.

Recommended context:
- Source of truth (code vs docs)
- Existing code entry points (directories, modules)
- Existing docs you trust (paths)
- Constraints and invariants you must preserve

**Example format:**
```
cf-constructor make DESIGN
Context:
- Source of truth: code
- Code areas: src/api/, src/domain/
- Existing docs: docs/architecture.md (may be outdated)
- Constraints: do not break public API
```

The agent will read inputs, ask targeted questions, propose answers, and produce artifacts.

---

## Workflow: Create Baseline

Goal: produce validated baseline artifacts before you add or refactor features.

### 1. PRD

**Reverse-engineer from code**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor reverse PRD from codebase` | Extracts requirements from existing code |
| `cf-constructor reverse PRD from src/` | Extracts from specific directory |
| `cf-constructor make PRD from code` | Same, alternative phrasing |

**Create from existing docs**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make PRD from README` | Creates PRD from project README |
| `cf-constructor make PRD from docs/requirements.txt` | Extracts from existing requirements |
| `cf-constructor make PRD from this conversation` | Creates PRD from stakeholder discussion |
| `cf-constructor import user-stories.md as PRD` | Converts user stories to PRD |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update PRD` | Updates PRD interactively |
| `cf-constructor extend PRD with {capability}` | Adds capability to existing PRD |
| `cf-constructor sync PRD from code` | Updates PRD to match current code |

**Provide context:** source of truth, code entry points, existing docs.

**Example:**
```
cf-constructor reverse PRD from codebase
Context:
- Source of truth: code
- Code entry points: src/routes/, src/controllers/
- Existing docs: docs/api.md (partial)
```

### 2. Validate PRD

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate PRD` | Full validation (300+ criteria) |
| `cf-constructor validate PRD semantic` | Semantic only (content quality) |
| `cf-constructor validate PRD structural` | Structural only (format, IDs) |
| `cf-constructor validate PRD quick` | Fast check (critical issues) |

### 3. ADR + DESIGN

**Reverse-engineer DESIGN**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor reverse DESIGN from codebase` | Documents current architecture from code |
| `cf-constructor reverse DESIGN from src/` | From specific directory |
| `cf-constructor make DESIGN from code` | Same, alternative phrasing |

**Create from existing docs**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make DESIGN from PRD` | Transforms PRD into architecture |
| `cf-constructor import OpenAPI as DESIGN` | Converts API spec into DESIGN |
| `cf-constructor import db-schema.sql as DESIGN data model` | Extracts data model from SQL |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update DESIGN` | Updates DESIGN interactively |
| `cf-constructor extend DESIGN with {component}` | Adds component to DESIGN |
| `cf-constructor sync DESIGN from code` | Updates DESIGN to match current code |

**ADR**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make ADR for PostgreSQL` | Creates ADR for technology choice |
| `cf-constructor make ADR for REST vs GraphQL` | Creates ADR comparing approaches |
| `cf-constructor draft ADR from discussion` | Extracts decision from conversation |
| `cf-constructor update ADR 0001` | Updates specific ADR |

**Provide context:** source of truth, existing features, constraints.

**Example:**
```
cf-constructor reverse DESIGN from codebase
Context:
- Source of truth: code
- Existing features: docs/openapi.yaml, docs/db-schema.md
- Constraints: do not break public API
```

### 4. Validate DESIGN + ADR

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate DESIGN` | Full validation (380+ criteria) |
| `cf-constructor validate DESIGN semantic` | Semantic only (consistency) |
| `cf-constructor validate DESIGN structural` | Structural only (format) |
| `cf-constructor validate DESIGN refs` | Cross-references to PRD |
| `cf-constructor validate ADR` | Validates all ADRs |
| `cf-constructor validate ADR 0001` | Validates specific ADR |
| `cf-constructor validate ADR semantic` | Semantic only (rationale quality) |

### 5. DECOMPOSITION

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor decompose` | Creates DECOMPOSITION interactively |
| `cf-constructor decompose from codebase` | Extracts features from existing code structure |
| `cf-constructor decompose by module` | Groups by code modules |
| `cf-constructor decompose by capability` | Groups by business capability |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor add feature {slug}` | Adds new feature entry |
| `cf-constructor update feature {slug} status` | Updates feature status |
| `cf-constructor update feature {slug} priority` | Updates feature priority |

**Provide context:** module boundaries, feature grouping preferences.

**Example:**
```
cf-constructor decompose from codebase
Context:
- Group features by modules: billing, auth, reporting
- Code structure: src/modules/
```

### 6. Validate DECOMPOSITION

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate DECOMPOSITION` | Full validation (130+ criteria) |
| `cf-constructor validate DECOMPOSITION semantic` | Semantic only (coverage) |
| `cf-constructor validate DECOMPOSITION structural` | Structural only (format) |
| `cf-constructor validate DECOMPOSITION refs` | Cross-references to DESIGN |

---

## Workflow: Add a New Feature

Use when baseline exists and you want to add a new capability.

### 1. Add to DECOMPOSITION

| Prompt | What happens |
|--------|--------------|
| `cf-constructor add feature {slug}` | Adds new feature to decomposition |
| `cf-constructor add feature notifications` | Example: adds notifications feature |

### 2. FEATURE

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make FEATURE for {slug}` | Creates FEATURE |
| `cf-constructor make FEATURE for notifications` | Creates detailed feature design |

**Reverse-engineer from existing code**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor reverse FEATURE from src/notifications/` | Creates FEATURE from existing code |
| `cf-constructor reverse FEATURE {slug} from code` | Same, using feature slug |
| `cf-constructor draft FEATURE from code` | Same, alternative phrasing |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update FEATURE {slug}` | Updates FEATURE |
| `cf-constructor extend FEATURE {slug} with {scenario}` | Adds scenario to FEATURE |
| `cf-constructor sync FEATURE {slug} from code` | Updates FEATURE to match code |

**Provide context:** feature slug, code boundaries, scenarios to include.

**Example:**
```
cf-constructor make FEATURE for notifications
Context:
- Include scenarios: retries, rate limits, provider outage
- Code boundaries: src/notifications/
```

### 3. Validate FEATURE

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate FEATURE {slug}` | Full validation (380+ criteria) |
| `cf-constructor validate FEATURE {slug} semantic` | Semantic only (flows, edge cases) |
| `cf-constructor validate FEATURE {slug} structural` | Structural only (CDSL, IDs) |
| `cf-constructor validate FEATURE {slug} refs` | Cross-references to DESIGN |

### 4. CODE

**Implement from scratch**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor implement {slug}` | Generates code from FEATURE |
| `cf-constructor implement {slug} step by step` | Implements with user confirmation |
| `cf-constructor implement {slug} tests first` | Generates tests first, then code |

**Implement specific parts**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor implement {slug} flow {flow-id}` | Implements specific flow only |
| `cf-constructor implement {slug} api` | Implements API layer only |
| `cf-constructor implement {slug} data layer` | Implements data/repository layer only |
| `cf-constructor implement {slug} tests` | Generates tests only |

**Continue / update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor continue implementing {slug}` | Continues partial implementation |
| `cf-constructor implement {slug} remaining` | Implements only unimplemented parts |
| `cf-constructor sync code with FEATURE {slug}` | Updates code to match FEATURE changes |

**Add markers to existing code**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor add markers to {path}` | Adds `@cpt-*` markers to existing code |
| `cf-constructor add markers for {slug}` | Adds markers matching FEATURE |
| `cf-constructor fix markers in {path}` | Fixes incorrect/incomplete markers |

**Provide context:** feature slug, code paths.

**Example:**
```
cf-constructor implement notifications
Context:
- Where to implement: src/notifications/
```

### 5. Validate Code

**Full validation**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate code` | Validates all code markers |
| `cf-constructor validate code for {slug}` | Validates specific feature |
| `cf-constructor validate code in {path}` | Validates code in specific path |

**Coverage**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate code coverage` | Reports implementation coverage % |
| `cf-constructor validate code coverage for {slug}` | Coverage for specific feature |
| `cf-constructor show uncovered flows` | Lists flows without implementation |
| `cf-constructor show uncovered algorithms` | Lists algorithms without implementation |

**Traceability**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate code orphans` | Finds markers referencing non-existent IDs |
| `cf-constructor validate code refs` | Validates all marker references |
| `cf-constructor validate code markers` | Checks marker format correctness |
| `cf-constructor list code markers` | Lists all markers in codebase |
| `cf-constructor list code markers for {slug}` | Lists markers for specific feature |

**Consistency**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor compare code to FEATURE {slug}` | Shows drift between code and feature |
| `cf-constructor validate code consistency` | Checks code matches FEATURE |
| `cf-constructor find missing implementations` | Lists FEATURE elements without code |

---

## Sync and Compare

When code and design drift apart:

| Prompt | What happens |
|--------|--------------|
| `cf-constructor compare DESIGN to code` | Shows drift between design and implementation |
| `cf-constructor compare FEATURE {slug} to code` | Shows drift for specific feature |
| `cf-constructor sync DESIGN from code` | Updates DESIGN to match current code |
| `cf-constructor sync FEATURE {slug} from code` | Updates FEATURE to match current code |
| `cf-constructor sync code with FEATURE {slug}` | Updates code to match FEATURE |
| `cf-constructor diff FEATURE {slug}` | Shows changes since last validation |

---

## Common Scenarios

### Requirements Changed

```
cf-constructor update PRD
cf-constructor validate PRD
cf-constructor propagate PRD changes to DESIGN
cf-constructor validate DESIGN
```

### Design Changed

```
cf-constructor update DESIGN
cf-constructor validate DESIGN
cf-constructor propagate DESIGN changes to FEATURE {slug}
cf-constructor validate FEATURE {slug}
```

### Feature Design Changed

```
cf-constructor update FEATURE {slug}
cf-constructor validate FEATURE {slug}
cf-constructor sync code with FEATURE {slug}
cf-constructor validate code for {slug}
```

### Code Changed Without Design Update

```
cf-constructor compare FEATURE {slug} to code
cf-constructor sync FEATURE {slug} from code
cf-constructor validate FEATURE {slug}
```

---

## Quick Reference

### By Adoption Level

| Level | What you do | Benefits |
|-------|-------------|----------|
| **Code-only** | `cf-constructor implement {slug}` | Code checklist, consistent patterns |
| **+ FEATURE** | Add `cf-constructor make FEATURE` | Flows, algorithms, edge cases documented |
| **+ DECOMPOSITION** | Add `cf-constructor decompose` | Feature organization, dependencies |
| **+ DESIGN** | Add `cf-constructor make DESIGN` | Architecture, components, data model |
| **+ PRD** | Add `cf-constructor make PRD` | Requirements, actors, full traceability |

### Top-Down (Full)

| Step | Generate | Validate |
|------|----------|----------|
| 1 | `cf-constructor reverse PRD from codebase` | `cf-constructor validate PRD` |
| 2 | `cf-constructor reverse DESIGN from codebase` | `cf-constructor validate DESIGN` |
| 3 | `cf-constructor decompose` | `cf-constructor validate DECOMPOSITION` |
| 4 | `cf-constructor make FEATURE for {slug}` | `cf-constructor validate FEATURE {slug}` |
| 5 | `cf-constructor implement {slug}` | `cf-constructor validate code for {slug}` |

### Bottom-Up (Feature-First)

| Step | Generate | Validate |
|------|----------|----------|
| 1 | `cf-constructor reverse FEATURE from src/{path}/` | `cf-constructor validate FEATURE {slug}` |
| 2 | `cf-constructor decompose from features` | `cf-constructor validate DECOMPOSITION` |
| 3 | `cf-constructor make DESIGN from DECOMPOSITION` | `cf-constructor validate DESIGN` |

### Code-Only

| Prompt | What happens |
|--------|--------------|
| `cf-constructor implement {slug}` | Generates code with checklist guidance |
| `cf-constructor add markers to {path}` | Adds traceability to existing code |
| `cf-constructor validate code` | Validates code quality |

**Validation modes** (append to any `validate` command):
- `semantic` — content quality, completeness, clarity
- `structural` — format, IDs, template compliance
- `refs` — cross-references to other artifacts
- `quick` — critical issues only (fast)

## Iteration Rules

- Start with what you need — add more artifacts as value becomes clear
- If code changes affect feature behavior, update FEATURE first
- Re-validate the FEATURE design
- Run `cf-constructor validate code` to ensure design and code remain consistent
- If code contradicts design, decide: update design OR update code
