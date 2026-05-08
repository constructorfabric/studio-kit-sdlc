---
name: cf-constructor-sdlc
description: "Artifacts: ADR, CODEBASE, DECOMPOSITION, DESIGN, FEATURE, PR-CODE-REVIEW-TEMPLATE, PR-REVIEW, PR-STATUS-REPORT-TEMPLATE, PRD; Workflows: migrate-openspec, pr-review, pr-status"
---

# Cyber Constructor Skill — Kit `sdlc`

Kit `sdlc` skill extensions.

## ADR

### ADR Commands
- `cfc validate --artifact <ADR.md>` — validate ADR structure and IDs
- `cfc list-ids --kind adr` — list all ADRs
- `cfc where-defined --id <id>` — find where an ADR ID is defined
- `cfc where-used --id <id>` — find where an ADR ID is referenced in DESIGN
### ADR Workflows
- **Generate ADR**: create a new ADR from template with guided prompts per section
- **Analyze ADR**: validate structure (deterministic) then semantic quality (checklist-based)

## CODEBASE

### CODE Commands
- `cfc validate --artifact <code-path>` — validate code traceability and quality
- `cfc where-defined --id <id>` — find where an ID is defined in artifacts
- `cfc where-used --id <id>` — find where an ID is referenced in code via `@cpt-*` markers
### CODE Workflows
- **Generate CODE**: implement FEATURE design with optional `@cpt-*` traceability markers
- **Analyze CODE**: validate implementation coverage, traceability, tests, and quality

## DECOMPOSITION

### DECOMPOSITION Commands
- `cfc validate --artifact <DECOMPOSITION.md>` — validate DECOMPOSITION structure and IDs
- `cfc list-ids --kind feature` — list all features
- `cfc list-ids --kind status` — list status indicators
- `cfc where-defined --id <id>` — find where a feature ID is defined
- `cfc where-used --id <id>` — find where a feature ID is referenced in FEATURE artifacts
### DECOMPOSITION Workflows
- **Generate DECOMPOSITION**: create feature manifest from DESIGN with guided prompts
- **Analyze DECOMPOSITION**: validate structure (deterministic) then decomposition quality (checklist-based)

## DESIGN

### DESIGN Commands
- `cfc validate --artifact <DESIGN.md>` — validate DESIGN structure and IDs
- `cfc list-ids --kind component` — list all components
- `cfc list-ids --kind principle` — list all design principles
- `cfc where-defined --id <id>` — find where a DESIGN ID is defined
- `cfc where-used --id <id>` — find where a DESIGN ID is referenced downstream
### DESIGN Workflows
- **Generate DESIGN**: create a new DESIGN from template with guided prompts per section
- **Analyze DESIGN**: validate structure (deterministic) then semantic quality (checklist-based)

## FEATURE

### FEATURE Commands
- `cfc validate --artifact <FEATURE.md>` — validate FEATURE structure and IDs
- `cfc list-ids --kind flow` — list all flows
- `cfc list-ids --kind algo` — list all algorithms
- `cfc list-ids --kind state` — list all state machines
- `cfc list-ids --kind dod` — list all definitions of done
- `cfc where-defined --id <id>` — find where a FEATURE ID is defined
- `cfc where-used --id <id>` — find where a FEATURE ID is referenced in code
### FEATURE Workflows
- **Generate FEATURE**: create a new FEATURE from template with guided CDSL prompts
- **Analyze FEATURE**: validate structure (deterministic) then semantic quality (checklist-based)

## PR-REVIEW

## PR Review & Status (Shortcut Routing)

ALWAYS re-fetch and re-analyze from scratch WHEN a PR review or status request is detected — even if the same PR was reviewed earlier in this conversation. Previous results are stale the moment a new request arrives. NEVER skip fetch or reuse earlier analysis.

ALWAYS run `python3 {scripts}/pr.py list` WHEN user intent matches PR list patterns:
- `list PRs`, `list open PRs`, `cf-constructor list PRs`
- `show PRs`, `show open PRs`, `what PRs are open`
- Any request to enumerate or browse open pull requests

AVOID use `gh pr list` directly — ALWAYS use `pr.py list` for listing PRs.

ALWAYS route to the `cf-constructor-pr-review` workflow WHEN user intent matches PR review patterns:
- `review PR {number}`, `review PR #{number}`, `review PR https://...`
- `cf-constructor review PR {number}`, `PR review {number}`
- `code review PR {number}`, `check PR {number}`

ALWAYS route to the `cf-constructor-pr-status` workflow WHEN user intent matches PR status patterns:
- `PR status {number}`, `cf-constructor PR status {number}`
- `status of PR {number}`, `check PR status {number}`

### PR List (Quick Command)

When routed to list PRs:
1. Run `python3 {scripts}/pr.py list`
2. Present the output to the user (respects `.prs/config.yaml` exclude list)
3. No Protocol Guard or workflow loading required — this is a quick command

### PR Review Workflow

When routed to PR review:
1. **ALWAYS fetch fresh data first** — run `pr.py fetch` even if data exists from a prior run
2. Read `{workflow_pr_review}` and follow its steps
3. Use `python3 {scripts}/pr.py` as the script
4. When target is `ALL` or no PR number given, run `pr.py list` first to show available PRs
5. Select prompt and checklist from `{cf-constructor-path}/config/pr-review.toml` → `prompts`
6. Load prompt from `prompt_file` and checklist from `checklist` in matched entry
7. Use templates from `{pr_code_review_template}` and `{pr_status_report_template}`

### PR Status Workflow

When routed to PR status:
1. **ALWAYS fetch fresh data first** — `pr.py status` auto-fetches, but never assume prior data is current
2. Read `{workflow_pr_status}` and follow its steps
3. Use `python3 {scripts}/pr.py` as the script
4. When target is `ALL` or no PR number given, run `pr.py list` first to show available PRs

## MIGRATION

### Migration Commands
- `cf-constructor migrate-openspec` — migrate OpenSpec artifacts to Cyber Constructor SDLC documents

### Migration Workflows

ALWAYS route to the `cf-constructor-migrate-openspec` workflow WHEN user intent matches OpenSpec migration patterns:
- `migrate openspec`, `migrate from openspec`, `convert openspec`
- `cf-constructor migrate-openspec`, `openspec to Cyber Constructor`
- Any request to convert OpenSpec artifacts to Cyber Constructor SDLC format

When routed to OpenSpec migration:
1. Read `{workflow_migrate_openspec}` and follow its steps
2. The workflow handles all configuration discovery and user interaction

## PRD

### PRD Commands
- `cfc validate --artifact <PRD.md>` — validate PRD structure and IDs
- `cfc list-ids --kind fr` — list all functional requirements
- `cfc list-ids --kind actor` — list all actors
- `cfc where-defined --id <id>` — find where a PRD ID is defined
- `cfc where-used --id <id>` — find where a PRD ID is referenced downstream
### PRD Workflows
- **Generate PRD**: create a new PRD from template with guided prompts per section
- **Analyze PRD**: validate structure (deterministic) then semantic quality (checklist-based)
