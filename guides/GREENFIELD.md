# Greenfield Guide

Use this guide when you are starting a new project from scratch.

All prompts work through the `cf-studio` skill — enable it with `cf-studio on` and use natural language prompts.

## Goal

Create a validated baseline (PRD + architecture) before writing code.

## What You Will Produce

Constructor Studio artifacts registered in `{cf-studio-path}/config/artifacts.toml` ([taxonomy](TAXONOMY.md)):

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
| `cf-studio set artifacts_dir to docs/design/` | Changes default base directory for new artifacts |
| `cf-studio move PRD to docs/requirements/PRD.md` | Moves artifact to new location |
| `cf-studio register PRD at docs/requirements/product-requirements.md` | Registers existing file as PRD artifact |
| `cf-studio show artifact locations` | Displays current paths from `artifacts.toml` |

You can also edit `{cf-studio-path}/config/artifacts.toml` directly:
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
cf-studio make DESIGN for taskman
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
| `cf-studio make PRD` | Creates PRD interactively |
| `cf-studio make PRD for taskman` | Creates PRD with context |
| `cf-studio draft PRD from README` | Extracts initial PRD from README |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update PRD` | Updates PRD interactively |
| `cf-studio extend PRD with task labels` | Adds capability to existing PRD |
| `cf-studio update PRD actors` | Updates actors section |
| `cf-studio update PRD requirements` | Updates requirements section |

**Provide context:** product vision, target users, key capabilities, existing PRD/BRD.

**Example:**
```
cf-studio make PRD for taskman
Context:
- Product: taskman - CLI task management tool
- Users: developers, solo users
- Key capabilities: create tasks, list tasks, mark done, due dates, priorities
- Storage: SQLite local database
```

### 2. Validate PRD

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate PRD` | Full validation (300+ criteria) |
| `cf-studio validate PRD semantic` | Semantic only (content quality) |
| `cf-studio validate PRD structural` | Structural only (format, IDs) |
| `cf-studio validate PRD quick` | Fast check (critical issues) |

### 3. ADR + DESIGN

**Create DESIGN**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make DESIGN` | Creates DESIGN interactively |
| `cf-studio make DESIGN from PRD` | Transforms PRD into architecture |

**Update DESIGN**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update DESIGN` | Updates DESIGN interactively |
| `cf-studio extend DESIGN with sync-service` | Adds component to DESIGN |
| `cf-studio update DESIGN components` | Updates components section |
| `cf-studio update DESIGN data model` | Updates data model section |

**Create ADR**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make ADR for SQLite` | Creates ADR for technology choice |
| `cf-studio make ADR for CLI vs TUI` | Creates ADR comparing approaches |

**Update ADR**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update ADR 0001` | Updates specific ADR |
| `cf-studio supersede ADR 0001 with 0002` | Creates new ADR superseding old |

**Provide context:** architecture constraints, existing domain model, API contracts.

**Example:**
```
cf-studio make DESIGN for taskman
Context:
- Tech: CLI tool, SQLite storage
- Constraints: offline-first, single binary, cross-platform
- Language: Go or Rust
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
| `cf-studio decompose into features` | Creates ordered feature list |
| `cf-studio decompose by capability` | Groups by business capability |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio add feature labels` | Adds new feature entry |
| `cf-studio update feature task-crud status` | Updates feature status |
| `cf-studio update feature task-crud priority` | Updates feature priority |
| `cf-studio reorder features` | Changes feature order |

**Provide context:** feature boundaries, grouping preferences.

**Example:**
```
cf-studio decompose taskman into features
Context:
- Split by capability: task-crud, task-list, task-search, labels, sync
```

### 6. Validate DECOMPOSITION

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate DECOMPOSITION` | Full validation (130+ criteria) |
| `cf-studio validate DECOMPOSITION semantic` | Semantic only (coverage) |
| `cf-studio validate DECOMPOSITION structural` | Structural only (format) |
| `cf-studio validate DECOMPOSITION refs` | Cross-references to DESIGN |

### 7. FEATURE

**Create**

| Prompt | What happens |
|--------|--------------|
| `cf-studio make FEATURE for task-crud` | Creates feature design |
| `cf-studio make FEATURE for task-list` | Creates detailed feature design |

**Update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio update FEATURE task-crud` | Updates feature design |
| `cf-studio extend FEATURE task-crud with bulk-delete` | Adds scenario to feature |
| `cf-studio update FEATURE task-crud flows` | Updates flows section |
| `cf-studio update FEATURE task-crud algorithms` | Updates algorithms section |

**Provide context:** feature slug, acceptance criteria, edge cases, error handling.

**Example:**
```
cf-studio make FEATURE for task-crud
Context:
- Include scenarios: create, update, delete, bulk operations
- Edge cases: duplicate titles, invalid dates, missing required fields
```

### 8. Validate FEATURE

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate FEATURE task-crud` | Full validation (380+ criteria) |
| `cf-studio validate FEATURE task-crud semantic` | Semantic only (flows, edge cases) |
| `cf-studio validate FEATURE task-crud structural` | Structural only (CDSL, IDs) |
| `cf-studio validate FEATURE task-crud refs` | Cross-references to DESIGN |

### 9. CODE

**Implement from scratch**

| Prompt | What happens |
|--------|--------------|
| `cf-studio implement task-crud` | Generates code from FEATURE |
| `cf-studio implement feature task-crud` | Same, explicit feature keyword |
| `cf-studio implement task-crud step by step` | Implements with user confirmation at each step |
| `cf-studio implement task-crud tests first` | Generates tests first, then implementation |

**Implement specific parts**

| Prompt | What happens |
|--------|--------------|
| `cf-studio implement task-crud flow create` | Implements specific flow only |
| `cf-studio implement task-crud algorithm validate` | Implements specific algorithm only |
| `cf-studio implement task-crud api` | Implements API layer only |
| `cf-studio implement task-crud data layer` | Implements data/repository layer only |
| `cf-studio implement task-crud tests` | Generates tests only |

**Continue / update**

| Prompt | What happens |
|--------|--------------|
| `cf-studio continue implementing task-crud` | Continues partial implementation |
| `cf-studio sync code with FEATURE task-crud` | Updates code to match FEATURE changes |
| `cf-studio implement task-crud remaining` | Implements only unimplemented parts |
| `cf-studio refactor task-crud` | Refactors implementation keeping markers |

**Add markers to existing code**

| Prompt | What happens |
|--------|--------------|
| `cf-studio add markers to cmd/task.go` | Adds `@cpt-*` markers to existing code |
| `cf-studio add markers for task-crud` | Adds markers matching FEATURE |
| `cf-studio fix markers in cmd/` | Fixes incorrect/incomplete markers |

**Provide context:** feature slug, code paths if non-standard.

### 10. Validate Code

**Full validation**

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate code` | Validates all code markers |
| `cf-studio validate code for task-crud` | Validates specific feature |
| `cf-studio validate code in cmd/` | Validates code in specific path |

**Coverage**

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate code coverage` | Reports implementation coverage % |
| `cf-studio validate code coverage for task-crud` | Coverage for specific feature |
| `cf-studio show uncovered flows` | Lists flows without implementation |
| `cf-studio show uncovered algorithms` | Lists algorithms without implementation |

**Traceability**

| Prompt | What happens |
|--------|--------------|
| `cf-studio validate code orphans` | Finds markers referencing non-existent IDs |
| `cf-studio validate code refs` | Validates all marker references |
| `cf-studio validate code markers` | Checks marker format correctness |
| `cf-studio list code markers` | Lists all markers in codebase |
| `cf-studio list code markers for task-crud` | Lists markers for specific feature |

**Consistency**

| Prompt | What happens |
|--------|--------------|
| `cf-studio compare code to FEATURE task-crud` | Shows drift between code and feature |
| `cf-studio validate code consistency` | Checks code matches FEATURE |
| `cf-studio find missing implementations` | Lists FEATURE elements without code |

---

## Iteration Rules

- If a change impacts behavior, update the relevant design first (DESIGN or FEATURE)
- Re-run validation for the modified artifact before continuing
- If code contradicts design, update design first, then re-validate

## Quick Reference

| Step | Generate | Validate |
|------|----------|----------|
| 1 | `cf-studio make PRD for taskman` | `cf-studio validate PRD` |
| 2 | `cf-studio make DESIGN` | `cf-studio validate DESIGN` |
| 3 | `cf-studio make ADR for SQLite` | `cf-studio validate ADR` |
| 4 | `cf-studio decompose` | `cf-studio validate DECOMPOSITION` |
| 5 | `cf-studio make FEATURE for task-crud` | `cf-studio validate FEATURE task-crud` |
| 6 | `cf-studio implement task-crud` | `cf-studio validate code for task-crud` |

**Validation modes** (append to any `validate` command):
- `semantic` — content quality, completeness, clarity
- `structural` — format, IDs, template compliance
- `refs` — cross-references to other artifacts
- `quick` — critical issues only (fast)
