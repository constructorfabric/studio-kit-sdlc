# Brownfield Guide

Use this guide when you already have a codebase and want to adopt Constructor Studio.

All prompts work through the `cf-studio` skill — enable it with `cf-studio on` and use natural language prompts.

## Goal

Adopt Constructor Studio incrementally — start with what makes sense for your project, not a fixed sequence.

## Key Principle: Start Anywhere

Unlike greenfield projects, **brownfield has no required order**. You can:

- Start with **any artifact** — PRD, DESIGN, FEATURE, or even just CODE
- Work **top-down** (PRD → DESIGN → CODE) or **bottom-up** (CODE → FEATURE → DESIGN)
- Adopt **incrementally** — use only what you need, add more later
- Use **code-only mode** — just Constructor Studio's code generation with checklist benefits

**Even with zero artifacts**, Constructor Studio's code generation uses the `code-checklist` internally for quality guidance.

---

## Recommended First Step: Auto-Configuration

Before choosing an adoption scenario, run auto-config to let Constructor Studio learn your project:

| Prompt | What happens |
|--------|--------------|
| `cf-studio auto-config` | Scans project and generates rules, registry entries, and AGENTS.md integration |

**What it does** (6 phases):
1. **Project Scan** — reads directory structure, entry points, language/framework detection
2. **Documentation Discovery** — finds existing docs, READMEs, API specs, schemas
3. **System Detection** — identifies system boundaries, subsystems, components
4. **Rule Generation** — creates per-system convention rules in `{cf-studio-path}/config/rules/{slug}.md`
5. **AGENTS.md Integration** — adds WHEN rules to `{cf-studio-path}/config/AGENTS.md` so the agent loads the right rules at the right time
6. **Registry Update** — registers detected systems and codebase paths in `{cf-studio-path}/config/artifacts.toml`

**When to run**:
- After `cf-studio init` on an existing project
- After major structural changes (new modules, framework migration)
- When Constructor Studio doesn't know your conventions yet

**What you get**: The agent will now understand your project structure, naming conventions, tech stack, and test patterns — making all subsequent prompts (from any adoption scenario below) more accurate.

> **Note**: Auto-config is also triggered automatically by the `generate` workflow when it detects a brownfield project with no rules.

---

## Adoption Scenarios

### Scenario A: Code-Only

You just want better code generation. No artifacts needed.

| Prompt | What happens |
|--------|--------------|
| `cf-studio implement feature auth` | Generates code using code-checklist quality guidance |
| `cf-studio add markers to src/auth/` | Adds traceability markers to existing code |
| `cf-studio validate code` | Validates code quality and marker correctness |

**Benefits**: Quality-guided code generation, consistent patterns, code traceability.

### Scenario B: Feature-First (Bottom-Up)

You want to document existing features, then build up.

```
1. cf-studio reverse FEATURE from src/auth/     → Creates FEATURE from code
2. cf-studio reverse FEATURE from src/billing/  → Creates another FEATURE from code
3. cf-studio decompose from features            → Creates DECOMPOSITION from FEATUREs
4. cf-studio make DESIGN from DECOMPOSITION     → Creates DESIGN from structure
5. cf-studio make PRD from DESIGN               → (optional) Creates PRD from DESIGN
```

**When to use**: You want to document what exists without changing code.

### Scenario C: Design-First (Middle-Out)

You want to capture architecture, then decompose into features.

```
1. cf-studio reverse DESIGN from codebase       → Extracts architecture from code
2. cf-studio decompose                          → Creates feature breakdown
3. cf-studio make FEATURE for {slug}            → Creates detailed features
4. cf-studio implement {slug}                   → Implements with traceability markers
```

**When to use**: You want architectural control before feature work.


### Scenario D: Full Top-Down

You want complete documentation from requirements to code.

```
1. cf-studio reverse PRD from codebase          → Extracts requirements
2. cf-studio make DESIGN from PRD               → Creates architecture
3. cf-studio decompose                          → Creates feature breakdown
4. cf-studio make FEATURE for {slug}            → Creates detailed features
5. cf-studio implement {slug}                   → Implements with traceability markers
```

**When to use**: New team members, compliance requirements, or major refactoring.

### Scenario E: Gradual Adoption

Start minimal, add artifacts as needed.

```
Week 1: cf-studio implement {slug}              → Code-only, with checklist
Week 2: cf-studio make FEATURE for {slug}       → Add features for complex work
Week 3: cf-studio decompose                     → Organize features
Later:  cf-studio make DESIGN                   → Document architecture
```

**When to use**: You want low-friction adoption with growing benefits.

---

## What You Will Produce

Constructor Studio artifacts registered in `{cf-studio-path}/config/artifacts.toml` ([taxonomy](TAXONOMY.md)):

| Artifact | Default Location |
|----------|------------------|
| PRD | `architecture/PRD.md` |
| ADR | `architecture/ADR/*.md` |
| DESIGN | `architecture/DESIGN.md` |
| DECOMPOSITION | `architecture/DECOMPOSITION.md` |
| FEATURE | `architecture/features/{slug}.md` |

## How to Provide Context

Brownfield work is context-heavy. Add context to control what the agent reads and how it maps existing reality into Constructor Studio artifacts.

Recommended context:
- Source of truth (code vs docs)
- Existing code entry points (directories, modules)
- Existing docs you trust (paths)
- Constraints and invariants you must preserve

**Example format:**
```
cf-studio make DESIGN
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
| `cf-studio reverse PRD from codebase` | Extracts requirements from existing code |
| `cf-studio reverse PRD from src/` | Extracts from specific directory |
| `cf-studio make PRD from code` | Same, alternative phrasing |

**Create from existing docs**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make PRD from README` | Creates PRD from project README |
| `cf-studio make PRD from docs/requirements.txt` | Extracts from existing requirements |
| `cf-studio make PRD from this conversation` | Creates PRD from stakeholder discussion |
| `cf-studio import user-stories.md as PRD` | Converts user stories to PRD |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update PRD` | Updates PRD interactively |
| `cf-studio extend PRD with {capability}` | Adds capability to existing PRD |
| `cf-studio sync PRD from code` | Updates PRD to match current code |

**Provide context:** source of truth, code entry points, existing docs.

**Example:**
```
cf-studio reverse PRD from codebase
Context:
- Source of truth: code
- Code entry points: src/routes/, src/controllers/
- Existing docs: docs/api.md (partial)
```

### 2. Validate PRD

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate PRD` | Full validation (300+ criteria) |
| `cf-studio validate PRD semantic` | Semantic only (content quality) |
| `cf-studio validate PRD structural` | Structural only (format, IDs) |
| `cf-studio validate PRD quick` | Fast check (critical issues) |

### 3. ADR + DESIGN

**Reverse-engineer DESIGN**

| Prompt | What happens |
|--------|--------------|
| `cf-studio reverse DESIGN from codebase` | Documents current architecture from code |
| `cf-studio reverse DESIGN from src/` | From specific directory |
| `cf-studio make DESIGN from code` | Same, alternative phrasing |

**Create from existing docs**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make DESIGN from PRD` | Transforms PRD into architecture |
| `cf-studio import OpenAPI as DESIGN` | Converts API spec into DESIGN |
| `cf-studio import db-schema.sql as DESIGN data model` | Extracts data model from SQL |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update DESIGN` | Updates DESIGN interactively |
| `cf-studio extend DESIGN with {component}` | Adds component to DESIGN |
| `cf-studio sync DESIGN from code` | Updates DESIGN to match current code |

**ADR**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make ADR for PostgreSQL` | Creates ADR for technology choice |
| `cf-studio make ADR for REST vs GraphQL` | Creates ADR comparing approaches |
| `cf-studio draft ADR from discussion` | Extracts decision from conversation |
| `cf-studio update ADR 0001` | Updates specific ADR |

**Provide context:** source of truth, existing features, constraints.

**Example:**
```
cf-studio reverse DESIGN from codebase
Context:
- Source of truth: code
- Existing features: docs/openapi.yaml, docs/db-schema.md
- Constraints: do not break public API
```

### 4. Validate DESIGN + ADR

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate DESIGN` | Full validation (380+ criteria) |
| `cf-studio validate DESIGN semantic` | Semantic only (consistency) |
| `cf-studio validate DESIGN structural` | Structural only (format) |
| `cf-studio validate DESIGN refs` | Cross-references to PRD |
| `cf-studio validate ADR` | Validates all ADRs |
| `cf-studio validate ADR 0001` | Validates specific ADR |
| `cf-studio validate ADR semantic` | Semantic only (rationale quality) |

### 5. DECOMPOSITION

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-studio decompose` | Creates DECOMPOSITION interactively |
| `cf-studio decompose from codebase` | Extracts features from existing code structure |
| `cf-studio decompose by module` | Groups by code modules |
| `cf-studio decompose by capability` | Groups by business capability |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio add feature {slug}` | Adds new feature entry |
| `cf-studio update feature {slug} status` | Updates feature status |
| `cf-studio update feature {slug} priority` | Updates feature priority |

**Provide context:** module boundaries, feature grouping preferences.

**Example:**
```
cf-studio decompose from codebase
Context:
- Group features by modules: billing, auth, reporting
- Code structure: src/modules/
```

### 6. Validate DECOMPOSITION

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate DECOMPOSITION` | Full validation (130+ criteria) |
| `cf-studio validate DECOMPOSITION semantic` | Semantic only (coverage) |
| `cf-studio validate DECOMPOSITION structural` | Structural only (format) |
| `cf-studio validate DECOMPOSITION refs` | Cross-references to DESIGN |

---

## Workflow: Add a New Feature

Use when baseline exists and you want to add a new capability.

### 1. Add to DECOMPOSITION

| Prompt | What happens |
|--------|--------------|
| `cf-studio add feature {slug}` | Adds new feature to decomposition |
| `cf-studio add feature notifications` | Example: adds notifications feature |

### 2. FEATURE

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make FEATURE for {slug}` | Creates FEATURE |
| `cf-studio make FEATURE for notifications` | Creates detailed feature design |

**Reverse-engineer from existing code**

| Prompt | What happens |
|--------|--------------|
| `cf-studio reverse FEATURE from src/notifications/` | Creates FEATURE from existing code |
| `cf-studio reverse FEATURE {slug} from code` | Same, using feature slug |
| `cf-studio draft FEATURE from code` | Same, alternative phrasing |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update FEATURE {slug}` | Updates FEATURE |
| `cf-studio extend FEATURE {slug} with {scenario}` | Adds scenario to FEATURE |
| `cf-studio sync FEATURE {slug} from code` | Updates FEATURE to match code |

**Provide context:** feature slug, code boundaries, scenarios to include.

**Example:**
```
cf-studio make FEATURE for notifications
Context:
- Include scenarios: retries, rate limits, provider outage
- Code boundaries: src/notifications/
```

### 3. Validate FEATURE

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate FEATURE {slug}` | Full validation (380+ criteria) |
| `cf-studio validate FEATURE {slug} semantic` | Semantic only (flows, edge cases) |
| `cf-studio validate FEATURE {slug} structural` | Structural only (CDSL, IDs) |
| `cf-studio validate FEATURE {slug} refs` | Cross-references to DESIGN |

### 4. CODE

**Implement from scratch**

| Prompt | What happens |
|--------|--------------|
| `cf-studio implement {slug}` | Generates code from FEATURE |
| `cf-studio implement {slug} step by step` | Implements with user confirmation |
| `cf-studio implement {slug} tests first` | Generates tests first, then code |

**Implement specific parts**

| Prompt | What happens |
|--------|--------------|
| `cf-studio implement {slug} flow {flow-id}` | Implements specific flow only |
| `cf-studio implement {slug} api` | Implements API layer only |
| `cf-studio implement {slug} data layer` | Implements data/repository layer only |
| `cf-studio implement {slug} tests` | Generates tests only |

**Continue / update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio continue implementing {slug}` | Continues partial implementation |
| `cf-studio implement {slug} remaining` | Implements only unimplemented parts |
| `cf-studio sync code with FEATURE {slug}` | Updates code to match FEATURE changes |

**Add markers to existing code**

| Prompt | What happens |
|--------|--------------|
| `cf-studio add markers to {path}` | Adds `@cpt-*` markers to existing code |
| `cf-studio add markers for {slug}` | Adds markers matching FEATURE |
| `cf-studio fix markers in {path}` | Fixes incorrect/incomplete markers |

**Provide context:** feature slug, code paths.

**Example:**
```
cf-studio implement notifications
Context:
- Where to implement: src/notifications/
```

### 5. Validate Code

**Full validation**

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate code` | Validates all code markers |
| `cf-studio validate code for {slug}` | Validates specific feature |
| `cf-studio validate code in {path}` | Validates code in specific path |

**Coverage**

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate code coverage` | Reports implementation coverage % |
| `cf-studio validate code coverage for {slug}` | Coverage for specific feature |
| `cf-studio show uncovered flows` | Lists flows without implementation |
| `cf-studio show uncovered algorithms` | Lists algorithms without implementation |

**Traceability**

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate code orphans` | Finds markers referencing non-existent IDs |
| `cf-studio validate code refs` | Validates all marker references |
| `cf-studio validate code markers` | Checks marker format correctness |
| `cf-studio list code markers` | Lists all markers in codebase |
| `cf-studio list code markers for {slug}` | Lists markers for specific feature |

**Consistency**

| Prompt | What happens |
|--------|--------------|
| `cf-studio compare code to FEATURE {slug}` | Shows drift between code and feature |
| `cf-studio validate code consistency` | Checks code matches FEATURE |
| `cf-studio find missing implementations` | Lists FEATURE elements without code |

---

## Sync and Compare

When code and design drift apart:

| Prompt | What happens |
|--------|--------------|
| `cf-studio compare DESIGN to code` | Shows drift between design and implementation |
| `cf-studio compare FEATURE {slug} to code` | Shows drift for specific feature |
| `cf-studio sync DESIGN from code` | Updates DESIGN to match current code |
| `cf-studio sync FEATURE {slug} from code` | Updates FEATURE to match current code |
| `cf-studio sync code with FEATURE {slug}` | Updates code to match FEATURE |
| `cf-studio diff FEATURE {slug}` | Shows changes since last validation |

---

## Common Scenarios

### Requirements Changed

```
cf-studio update PRD
cf-studio validate PRD
cf-studio propagate PRD changes to DESIGN
cf-studio validate DESIGN
```

### Design Changed

```
cf-studio update DESIGN
cf-studio validate DESIGN
cf-studio propagate DESIGN changes to FEATURE {slug}
cf-studio validate FEATURE {slug}
```

### Feature Design Changed

```
cf-studio update FEATURE {slug}
cf-studio validate FEATURE {slug}
cf-studio sync code with FEATURE {slug}
cf-studio validate code for {slug}
```

### Code Changed Without Design Update

```
cf-studio compare FEATURE {slug} to code
cf-studio sync FEATURE {slug} from code
cf-studio validate FEATURE {slug}
```

---

## Quick Reference

### By Adoption Level

| Level | What you do | Benefits |
|-------|-------------|----------|
| **Code-only** | `cf-studio implement {slug}` | Code checklist, consistent patterns |
| **+ FEATURE** | Add `cf-studio make FEATURE` | Flows, algorithms, edge cases documented |
| **+ DECOMPOSITION** | Add `cf-studio decompose` | Feature organization, dependencies |
| **+ DESIGN** | Add `cf-studio make DESIGN` | Architecture, components, data model |
| **+ PRD** | Add `cf-studio make PRD` | Requirements, actors, full traceability |

### Top-Down (Full)

| Step | Generate | Validate |
|------|----------|----------|
| 1 | `cf-studio reverse PRD from codebase` | `cf-studio validate PRD` |
| 2 | `cf-studio reverse DESIGN from codebase` | `cf-studio validate DESIGN` |
| 3 | `cf-studio decompose` | `cf-studio validate DECOMPOSITION` |
| 4 | `cf-studio make FEATURE for {slug}` | `cf-studio validate FEATURE {slug}` |
| 5 | `cf-studio implement {slug}` | `cf-studio validate code for {slug}` |

### Bottom-Up (Feature-First)

| Step | Generate | Validate |
|------|----------|----------|
| 1 | `cf-studio reverse FEATURE from src/{path}/` | `cf-studio validate FEATURE {slug}` |
| 2 | `cf-studio decompose from features` | `cf-studio validate DECOMPOSITION` |
| 3 | `cf-studio make DESIGN from DECOMPOSITION` | `cf-studio validate DESIGN` |

### Code-Only

| Prompt | What happens |
|--------|--------------|
| `cf-studio implement {slug}` | Generates code with checklist guidance |
| `cf-studio add markers to {path}` | Adds traceability to existing code |
| `cf-studio validate code` | Validates code quality |

**Validation modes** (append to any `validate` command):
- `semantic` — content quality, completeness, clarity
- `structural` — format, IDs, template compliance
- `refs` — cross-references to other artifacts
- `quick` — critical issues only (fast)

## Iteration Rules

- Start with what you need — add more artifacts as value becomes clear
- If code changes affect feature behavior, update FEATURE first
- Re-validate the FEATURE design
- Run `cf-studio validate code` to ensure design and code remain consistent
- If code contradicts design, decide: update design OR update code
