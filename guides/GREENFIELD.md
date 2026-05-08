# Greenfield Guide

Use this guide when you are starting a new project from scratch.

All prompts work through the `cf-constructor` skill — enable it with `cf-constructor on` and use natural language prompts.

## Goal

Create a validated baseline (PRD + architecture) before writing code.

## What You Will Produce

Cyber Constructor artifacts registered in `{cf-constructor-path}/config/artifacts.toml` ([taxonomy](TAXONOMY.md)):

| Artifact | Default Location |
|----------|------------------|
| PRD | `architecture/PRD.md` |
| ADR | `architecture/ADR/*.md` |
| DESIGN | `architecture/DESIGN.md` |
| DECOMPOSITION | `architecture/DECOMPOSITION.md` |
| FEATURE | `architecture/features/{slug}.md` |

**Customizing artifact locations:**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor set artifacts_dir to docs/design/` | Changes default base directory for new artifacts |
| `cf-constructor move PRD to docs/requirements/PRD.md` | Moves artifact to new location |
| `cf-constructor register PRD at docs/requirements/product-requirements.md` | Registers existing file as PRD artifact |
| `cf-constructor show artifact locations` | Displays current paths from `artifacts.toml` |

You can also edit `{cf-constructor-path}/config/artifacts.toml` directly:
- `artifacts_dir` — Default base directory for new artifacts (default: `architecture`)
- Subdirectories for FEATUREs (`features/`) and ADRs (`ADR/`) are defined by the kit
- Artifact paths in `artifacts` array are FULL paths — you can place artifacts anywhere

## How to Provide Context

Each prompt can include additional context. Recommended:

- Current state (what exists, what is missing)
- Links/paths to existing docs (README, features, diagrams)
- Constraints (security, compliance, performance)
- Non-goals and out-of-scope items

**Example format:**
```
cf-constructor make DESIGN for taskman
Context:
- Repo: taskman - task management CLI tool
- Existing docs: README.md
- Constraints: SQLite storage, offline-first
```

The agent will read inputs, ask targeted questions, propose answers, and produce artifacts.

---

## Workflow Sequence

### 1. PRD

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make PRD` | Creates PRD interactively |
| `cf-constructor make PRD for taskman` | Creates PRD with context |
| `cf-constructor draft PRD from README` | Extracts initial PRD from README |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update PRD` | Updates PRD interactively |
| `cf-constructor extend PRD with task labels` | Adds capability to existing PRD |
| `cf-constructor update PRD actors` | Updates actors section |
| `cf-constructor update PRD requirements` | Updates requirements section |

**Provide context:** product vision, target users, key capabilities, existing PRD/BRD.

**Example:**
```
cf-constructor make PRD for taskman
Context:
- Product: taskman - CLI task management tool
- Users: developers, solo users
- Key capabilities: create tasks, list tasks, mark done, due dates, priorities
- Storage: SQLite local database
```

### 2. Validate PRD

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate PRD` | Full validation (300+ criteria) |
| `cf-constructor validate PRD semantic` | Semantic only (content quality) |
| `cf-constructor validate PRD structural` | Structural only (format, IDs) |
| `cf-constructor validate PRD quick` | Fast check (critical issues) |

### 3. ADR + DESIGN

**Create DESIGN**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make DESIGN` | Creates DESIGN interactively |
| `cf-constructor make DESIGN from PRD` | Transforms PRD into architecture |

**Update DESIGN**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update DESIGN` | Updates DESIGN interactively |
| `cf-constructor extend DESIGN with sync-service` | Adds component to DESIGN |
| `cf-constructor update DESIGN components` | Updates components section |
| `cf-constructor update DESIGN data model` | Updates data model section |

**Create ADR**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make ADR for SQLite` | Creates ADR for technology choice |
| `cf-constructor make ADR for CLI vs TUI` | Creates ADR comparing approaches |

**Update ADR**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update ADR 0001` | Updates specific ADR |
| `cf-constructor supersede ADR 0001 with 0002` | Creates new ADR superseding old |

**Provide context:** architecture constraints, existing domain model, API contracts.

**Example:**
```
cf-constructor make DESIGN for taskman
Context:
- Tech: CLI tool, SQLite storage
- Constraints: offline-first, single binary, cross-platform
- Language: Go or Rust
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
| `cf-constructor decompose into features` | Creates ordered feature list |
| `cf-constructor decompose by capability` | Groups by business capability |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor add feature labels` | Adds new feature entry |
| `cf-constructor update feature task-crud status` | Updates feature status |
| `cf-constructor update feature task-crud priority` | Updates feature priority |
| `cf-constructor reorder features` | Changes feature order |

**Provide context:** feature boundaries, grouping preferences.

**Example:**
```
cf-constructor decompose taskman into features
Context:
- Split by capability: task-crud, task-list, task-search, labels, sync
```

### 6. Validate DECOMPOSITION

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate DECOMPOSITION` | Full validation (130+ criteria) |
| `cf-constructor validate DECOMPOSITION semantic` | Semantic only (coverage) |
| `cf-constructor validate DECOMPOSITION structural` | Structural only (format) |
| `cf-constructor validate DECOMPOSITION refs` | Cross-references to DESIGN |

### 7. FEATURE

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor make FEATURE for task-crud` | Creates feature design |
| `cf-constructor make FEATURE for task-list` | Creates detailed feature design |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor update FEATURE task-crud` | Updates feature design |
| `cf-constructor extend FEATURE task-crud with bulk-delete` | Adds scenario to feature |
| `cf-constructor update FEATURE task-crud flows` | Updates flows section |
| `cf-constructor update FEATURE task-crud algorithms` | Updates algorithms section |

**Provide context:** feature slug, acceptance criteria, edge cases, error handling.

**Example:**
```
cf-constructor make FEATURE for task-crud
Context:
- Include scenarios: create, update, delete, bulk operations
- Edge cases: duplicate titles, invalid dates, missing required fields
```

### 8. Validate FEATURE

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate FEATURE task-crud` | Full validation (380+ criteria) |
| `cf-constructor validate FEATURE task-crud semantic` | Semantic only (flows, edge cases) |
| `cf-constructor validate FEATURE task-crud structural` | Structural only (CDSL, IDs) |
| `cf-constructor validate FEATURE task-crud refs` | Cross-references to DESIGN |

### 9. CODE

**Implement from scratch**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor implement task-crud` | Generates code from FEATURE |
| `cf-constructor implement feature task-crud` | Same, explicit feature keyword |
| `cf-constructor implement task-crud step by step` | Implements with user confirmation at each step |
| `cf-constructor implement task-crud tests first` | Generates tests first, then implementation |

**Implement specific parts**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor implement task-crud flow create` | Implements specific flow only |
| `cf-constructor implement task-crud algorithm validate` | Implements specific algorithm only |
| `cf-constructor implement task-crud api` | Implements API layer only |
| `cf-constructor implement task-crud data layer` | Implements data/repository layer only |
| `cf-constructor implement task-crud tests` | Generates tests only |

**Continue / update**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor continue implementing task-crud` | Continues partial implementation |
| `cf-constructor sync code with FEATURE task-crud` | Updates code to match FEATURE changes |
| `cf-constructor implement task-crud remaining` | Implements only unimplemented parts |
| `cf-constructor refactor task-crud` | Refactors implementation keeping markers |

**Add markers to existing code**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor add markers to cmd/task.go` | Adds `@cpt-*` markers to existing code |
| `cf-constructor add markers for task-crud` | Adds markers matching FEATURE |
| `cf-constructor fix markers in cmd/` | Fixes incorrect/incomplete markers |

**Provide context:** feature slug, code paths if non-standard.

### 10. Validate Code

**Full validation**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate code` | Validates all code markers |
| `cf-constructor validate code for task-crud` | Validates specific feature |
| `cf-constructor validate code in cmd/` | Validates code in specific path |

**Coverage**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate code coverage` | Reports implementation coverage % |
| `cf-constructor validate code coverage for task-crud` | Coverage for specific feature |
| `cf-constructor show uncovered flows` | Lists flows without implementation |
| `cf-constructor show uncovered algorithms` | Lists algorithms without implementation |

**Traceability**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor validate code orphans` | Finds markers referencing non-existent IDs |
| `cf-constructor validate code refs` | Validates all marker references |
| `cf-constructor validate code markers` | Checks marker format correctness |
| `cf-constructor list code markers` | Lists all markers in codebase |
| `cf-constructor list code markers for task-crud` | Lists markers for specific feature |

**Consistency**

| Prompt | What happens |
|--------|--------------|
| `cf-constructor compare code to FEATURE task-crud` | Shows drift between code and feature |
| `cf-constructor validate code consistency` | Checks code matches FEATURE |
| `cf-constructor find missing implementations` | Lists FEATURE elements without code |

---

## Iteration Rules

- If a change impacts behavior, update the relevant design first (DESIGN or FEATURE)
- Re-run validation for the modified artifact before continuing
- If code contradicts design, update design first, then re-validate

## Quick Reference

| Step | Generate | Validate |
|------|----------|----------|
| 1 | `cf-constructor make PRD for taskman` | `cf-constructor validate PRD` |
| 2 | `cf-constructor make DESIGN` | `cf-constructor validate DESIGN` |
| 3 | `cf-constructor make ADR for SQLite` | `cf-constructor validate ADR` |
| 4 | `cf-constructor decompose` | `cf-constructor validate DECOMPOSITION` |
| 5 | `cf-constructor make FEATURE for task-crud` | `cf-constructor validate FEATURE task-crud` |
| 6 | `cf-constructor implement task-crud` | `cf-constructor validate code for task-crud` |

**Validation modes** (append to any `validate` command):
- `semantic` — content quality, completeness, clarity
- `structural` — format, IDs, template compliance
- `refs` — cross-references to other artifacts
- `quick` — critical issues only (fast)
