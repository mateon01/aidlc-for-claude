# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI-DLC for Claude Code is a plugin implementing the AI-Driven Development Life Cycle methodology. It provides 19 slash commands and 19 specialized agents that guide structured three-phase software development (INCEPTION, CONSTRUCTION, OPERATIONS).

**Version**: 1.6.0 | **License**: Apache-2.0

## Build & Development

This is a **pure markdown plugin** -- no build step, no dependencies, no test framework.

- `package.json` contains metadata only (no scripts to run)
- Testing is done manually via live Claude Code sessions: install the plugin, run `/aidlc`, verify workflow
- GitHub Pages docs auto-deploy on push to `main` via `.github/workflows/deploy-docs.yml`
- To preview docs locally: `pip install mkdocs-material && mkdocs serve`

## Architecture

### Plugin System

- `plugin.json` has a `"skills"` field pointing to `./commands/` -- NO `"agents"` field (agents are auto-discovered by convention)
- **Commands** (`commands/`) = thin orchestration (10-30 lines each), define delegation and approval gates
- **Agents** (`agents/`) = detailed execution protocols (60-150 lines each), do the actual work
- The orchestrator `commands/aidlc.md` is the central file (~300 lines) that coordinates all stages

### Agent References

All agent references MUST use the fully qualified prefix: `aidlc-for-claude:aidlc-{agent-name}`. References without the prefix fail silently.

```
# Correct
Task(subagent_type="aidlc-for-claude:aidlc-requirements-analyst", model="opus", ...)

# WRONG - will fail silently
Task(subagent_type="aidlc-requirements-analyst", model="opus", ...)
```

### Agent Frontmatter

Agent `allowedTools:` is a **whitelist** -- tools not listed are blocked. All agents must include `AskUserQuestion` for interactive Q&A capability. Example:

```yaml
---
name: aidlc-functional-designer
description: "AI-DLC CONSTRUCTION Stage 1: Functional Design"
model: sonnet
allowedTools: Read, Write, Edit, Glob, Grep, AskUserQuestion
---
```

### Model Tiering

- **Opus**: Strategic reasoning (requirements, architecture, planning, code planning)
- **Sonnet**: Volume execution (design, code generation, testing, operations)
- **Haiku**: Fast detection (workspace scanning only -- `aidlc-workspace-analyst`)

### Three-Phase Workflow

```
INCEPTION (7 stages): Workspace Detection → [Scope Assessment] → Reverse Engineering
  → Requirements → User Stories → Workflow Planning → Application Design → Units

CONSTRUCTION (8 stages): System NFR → per-unit [sequential or parallel]
  (Functional Design → NFR Requirements → NFR Design → Infrastructure Design
  → Code Generation) → Build & Test

OPERATIONS (1 stage): Deployment Checklist + Developer README
```

### Key Patterns

- **Approval gates**: Every stage (except Workspace Detection) requires user approval via standardized 2-option format: "Request Changes" / "Continue to Next Stage"
- **State tracking**: `aidlc-docs/aidlc-state.md` tracks progress, fast-path, batch mode
- **Audit trail**: `aidlc-docs/audit.md` is append-only with ISO 8601 timestamps; managed by the orchestrator, not individual agents
- **Code location**: Application code at workspace root ONLY; `aidlc-docs/` for documentation only
- **Prerequisite validation**: Construction agents validate input files exist (Step 0) before proceeding
- **Deep questioning**: INCEPTION agents have mandatory question minimums (15-25 for requirements, 10 for design/stories, 8 for units) with multi-round follow-up
- **Parallel unit execution**: CONSTRUCTION supports concurrent unit processing for 3+ unit projects via parallel groups
- **Cross-unit context**: Unit N receives summaries of units 1 through N-1 for consistency
- **Fast paths**: Brownfield projects offer simple/complex/new-component/full scope options
- **Stage banners (MOTD)**: Every agent displays a formatted banner on start showing phase, stage number, agent name, model, and capabilities. The orchestrator also displays a stage banner before each delegation.
- **Batch approval**: Auto-approve design stages, review only code generation and build/test
- **PR review**: Standalone `/aidlc-review-pr` utility for analyzing PR diffs (code quality, security, performance, consistency) -- independent of the three-phase workflow
- **CI setup**: Standalone `/aidlc-ci-setup` utility for generating CI/CD pipelines, PR review workflows, and issue/PR templates -- detects tech stack automatically
- **Dependency graph**: Optional graph-based code dependency analysis with multi-backend support (File-based JSON, Neo4j local Docker, AWS Neptune). Opt-in during Workflow Planning with detailed backend configuration. Neo4j provides Cypher queries and browser visualization; Neptune supports IaC provisioning (CDK/Terraform/CloudFormation) and IAM auth. Deployment verification (9 checks) ensures graph DB health. Standalone `/aidlc-graph` utility available anytime. E2E verified with Neo4j backend (15-node/41-edge TypeScript project, all verification checks passed). Note: use MERGE (not CREATE) for idempotent Cypher operations; cypher-shell has JVM cold-start overhead (~1s) separate from actual query latency. Integrated pipeline: RE builds initial graph (brownfield), Code Gen updates incrementally per unit with post-update verification, Build & Test uses impact analysis for prioritized test execution (P1 direct, P2 1-hop, P3 full suite). Graph DB lifecycle (init, health checks, parallel coordination, teardown) managed by orchestrator. Graph operations are non-blocking — failures log warnings and continue the workflow.

## File Naming Conventions

- Commands: `aidlc-{stage-name}.md` (e.g., `aidlc-requirements-analysis.md`)
- Agents: `aidlc-{role-name}.md` (e.g., `aidlc-requirements-analyst.md`)
- Construction artifacts: `aidlc-docs/construction/{unit-name}/{stage}/`
- Code plans: `aidlc-docs/construction/plans/{unit-name}-code-generation-plan.md`

## Adding a New Stage

1. Create `commands/aidlc-{name}.md` with YAML frontmatter (`description:`) and delegation example
2. Create `agents/aidlc-{name}.md` with YAML frontmatter (`name:`, `description:`, `model:`, `allowedTools:`)
3. Update `commands/aidlc.md` orchestrator to include the new stage in the flow
4. Update `README.md` (commands table, agents table, ASCII diagram, plugin structure)
5. Update `docs/workflow.md`, `docs/commands.md`, `docs/getting-started.md`

## Documentation

- All README.md and GitHub Pages content MUST be in English
- MkDocs Material theme (standard edition, NOT Insiders) -- grid cards and some advanced features are unavailable
- `:material-icon:` syntax requires `pymdownx.emoji` extension (configured in `mkdocs.yml`)
- ASCII diagrams use only `+`, `-`, `|`, `^`, `v`, `<`, `>` characters -- no Unicode box-drawing

## Issue and Project Management

- **Issues**: All tasks, bugs, and feature requests MUST be registered as GitHub Issues before starting work
- **Projects**: All issues MUST be added to the appropriate GitHub Project board for tracking
- **Todo Lists**: Before beginning any work, ensure the task is tracked in both GitHub Issues and Projects
- Work should only commence after proper issue and project registration is complete


## Release Management Rule

- **Version Release Requirement**: Whenever the project version changes (major, minor, or patch), you **MUST** create a new GitHub release that matches the new version.
- **Agent & Role Documentation**: When adding a new agent role (such as in `aidlc-for-claude/CLAUDE.md`), update all related documentation (README, workflow docs, agent lists) to ensure the new role is properly described and integrated into the workflow and release notes.
