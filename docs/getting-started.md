# Getting Started

---

## Installation

!!! warning "Requirements"
    [Claude Code](https://claude.ai/code) v2.0 or later is required.

### Step 1: Add Marketplace

In the Claude Code chat, register the marketplace:

```
/plugin marketplace add mateon01/aidlc-for-claude
```

### Step 2: Install

Install the plugin from the marketplace:

```
/plugin install aidlc-for-claude
```

### Step 3: Verify

Confirm that `aidlc-for-claude` is listed as **enabled**:

```
/plugin list
```

---

## Quick Start

Launch Claude Code in your project directory and run:

```
/aidlc
```

!!! tip
    That's all you need. The orchestrator handles everything from here.

The orchestrator will:

1. **Detect your workspace** -- greenfield (empty) or brownfield (existing code)
2. **Gather requirements** -- ask what you want to build
3. **Create an execution plan** -- determine which stages are needed (including System NFR for multi-unit projects)
4. **Walk through each stage** -- with your approval at every step

---

## Brownfield Fast Path

When working with an existing codebase, AI-DLC detects it as brownfield and asks about the scope of your change:

- **Simple change** -- Bug fixes, small features, config changes. Skips most analysis stages and goes directly to code generation. Fastest path.
- **Complex change** -- Multi-component features or refactoring. Streamlined path that includes requirements analysis but skips user stories.
- **New component** -- Greenfield development within an existing repository. Adds a new component to existing system using full structured workflow.
- **Full workflow** -- Complete AI-DLC treatment. All stages evaluated based on your project's needs.

!!! tip
    For quick bug fixes, choose "Simple change" to skip the full analysis pipeline and get to code generation faster.

---

## Batch Approval Mode

After Workflow Planning, AI-DLC offers a batch approval option:

- **Stage-by-stage** (default) -- Review and approve each stage individually
- **Batch approve construction** -- Auto-approve all construction design stages (Functional Design through Infrastructure Design), and review only Code Generation and Build & Test results

!!! tip
    For large multi-unit projects, batch approval significantly reduces the number of review steps while still giving you control over the most critical stages (code and tests).

---

## Running Individual Stages

You can run any stage independently:

=== "Requirements"

    ```
    /aidlc-requirements-analysis
    ```

=== "System NFR"

    ```
    /aidlc-system-nfr
    ```

=== "Code Generation"

    ```
    /aidlc-code-generation
    ```

=== "Build & Test"

    ```
    /aidlc-build-and-test
    ```

!!! note
    This is useful when you want to re-run a specific stage or start from a particular point in the workflow.

---

## PR Review

AI-DLC includes a standalone PR review utility that can analyze code changes independently of the three-phase workflow.

=== "Review a GitHub PR"

    ```
    /aidlc-review-pr
    ```

    Select "Review a GitHub PR" and provide the PR number. The agent fetches the diff via `gh pr diff` and performs a 6-category analysis: correctness, security, performance, consistency, testing, and documentation.

=== "Review Local Changes"

    ```
    /aidlc-review-pr
    ```

    Select "Review local changes" to analyze uncommitted or staged changes in your working directory. Useful for pre-commit review.

!!! tip
    The PR review agent is read-only -- it analyzes code but does not modify files. No approval gate is required.

---

## CI Setup

Generate CI/CD infrastructure for any project without running the full AI-DLC workflow:

```
/aidlc-ci-setup
```

The agent automatically detects your tech stack (language, build system, test framework, linter) and offers to generate:

- **CI/CD Pipeline** -- GitHub Actions or GitLab CI with build, test, lint, and security audit
- **PR Review Workflow** -- AI-powered code review using Claude API or OpenAI API
- **Issue Templates** -- Feature request and bug report forms customized to your project
- **PR Template** -- Standardized pull request description with project-specific checklists

!!! tip
    This is the fastest way to add CI/CD to an existing project. No prior AI-DLC stages are required.

---

## Graph Analysis

Build and visualize code dependency graphs for any project:

```
/aidlc-graph
```

The agent detects your project's language (TypeScript/JavaScript, Python, or general) and offers nine modes:

- **Build graph** -- Full static analysis to construct dependency graph from scratch
- **Update graph** -- Incrementally update existing graph with recent changes
- **Visualize** -- Generate Mermaid diagram from existing graph
- **Export as PNG** -- Export dependency graph as PNG images (requires Python matplotlib + networkx)
- **Impact analysis** -- Show which modules are affected by recent file changes
- **Search (GraphRAG)** -- Find modules by semantic query using summaries and graph context (requires graphRAGEnabled)
- **Repair (CGIG)** -- Compilation-Guided Iterative Graph-retrieval for automated error resolution (requires cgigEnabled)
- **Verify** -- Test connectivity and data integrity of graph DB
- **Teardown** -- Stop graph DB container or clean up cloud resources

Three backends are supported:

- **Neo4j Local** (recommended) -- Docker-based with Cypher queries and browser visualization at localhost:7474
- **AWS Neptune** -- AWS managed graph DB with IaC provisioning (CDK/Terraform/CloudFormation) and IAM auth
- **File-based** -- Simple JSON file with no external dependencies

!!! tip "Integrated Workflow"
    When using the full AI-DLC workflow (`/aidlc`), you can opt-in to dependency graph analysis during Workflow Planning. A multi-tier configuration flow lets you choose the backend (Neo4j, Neptune, or File-based) and configure deployment verification. The graph is then automatically maintained throughout the pipeline:

    - **Reverse Engineering** builds the initial graph from existing code (brownfield projects)
    - **Code Generation** updates the graph incrementally per unit with post-update integrity verification (node count, circular dependency check, cross-unit edge resolution)
    - **Build & Test** uses impact analysis for prioritized test execution -- direct changes first (P1), 1-hop dependents second (P2), then the full suite (P3), and CGIG runs compilation repair loop when cgigEnabled

    The orchestrator manages graph DB lifecycle: initialization before the first graph operation, health checks between stages, serialized updates during parallel execution, and a teardown offer at workflow completion. All graph operations are non-blocking -- failures log warnings and continue the workflow. You can retry any graph operation later with `/aidlc-graph`.

    **GraphRAG (optional):** When `graphRAGEnabled: true` is selected during Workflow Planning, module summaries (purpose, keywords, architectural layer) and community structure are generated alongside the dependency graph. This enables semantic code search via `/aidlc-graph search` -- finding modules by what they do, not just their file path. No external embedding models or vector databases required.

    **CGIG (optional):** When you select a CGIG or Hybrid graph construction method during Workflow Planning, the dependency graph is enriched with class-level properties (constructors, methods, fields, type hierarchy). If compilation fails during Build & Test, a repair loop queries the graph for fix suggestions:

    1. Compilation errors are parsed and classified into 10 categories (cannot_find_symbol, incompatible_types, missing_method, etc.)
    2. Per-category graph queries find relevant modules, methods, and type relationships
    3. Each suggestion gets a confidence score (0.2--0.9)
    4. High-confidence fixes are applied automatically; low-confidence ones are skipped
    5. The process repeats until compilation succeeds or max rounds are exhausted

    Four graph construction methods are available:

    - **Static** (default) -- Standard dependency graph with exports, imports, LOC
    - **CGIG** -- Enriched with constructors, methods, fields, type hierarchy
    - **Lightweight** -- Import-only graph, fastest for large codebases
    - **Hybrid** -- Static base, CGIG enrichment added on-demand when errors occur

!!! note "Graph Storage"
    File-based graphs are stored in `aidlc-docs/graph/dependency-graph.json`. Neo4j and Neptune backends store data in the graph database with a local summary at `aidlc-docs/graph/graph-summary.md`. Mermaid visualizations are generated at `aidlc-docs/graph/dependency-graph.md`. PNG exports (three views: full graph, community architecture, impact analysis) are saved to `aidlc-docs/graph/`. Neptune IaC files go to `aidlc-docs/graph/infra/`.

!!! success "Deployment Verification"
    After graph construction, a 9-point verification suite runs automatically:

    1. **Connection test** -- DB connectivity and response time
    2. **Schema validation** -- Uniqueness constraints applied
    3. **Node count** -- All modules loaded correctly
    4. **Edge count** -- All import relationships loaded
    5. **Orphan edge detection** -- No edges pointing to non-existent nodes
    6. **Duplicate edge detection** -- No duplicate relationships
    7. **Hub node analysis** -- Identifies critical dependency hubs
    8. **Circular dependency detection** -- Finds import cycles
    9. **Impact analysis** -- Calculates change impact radius for critical modules

    Results are saved to `aidlc-docs/graph/verification-report.md`.

!!! warning "Neptune CloudFormation Cleanup"
    When tearing down a Neptune backend provisioned via CloudFormation, stack deletion may be blocked by orphaned VPC Endpoint ENIs or GuardDuty-managed security groups. Delete these resources manually before retrying `aws cloudformation delete-stack`. Run `/aidlc-graph teardown` for detailed cleanup commands.

---

## Session Continuity

If your session is interrupted, simply run `/aidlc` again. The orchestrator detects the existing state file (`aidlc-docs/aidlc-state.md`) and offers to resume from where you left off.

---

## How Questions Work

AI-DLC uses a **hybrid questioning system** to gather thorough requirements:

**Interactive Q&A (AskUserQuestion)** -- For high-impact decisions that need immediate answers:

- Tech stack, database, authentication, deployment target, MVP scope
- Clickable multiple-choice options directly in the terminal
- Each question offers recommended defaults you can accept with one click

**Document-based questionnaires (.md files)** -- For detailed analysis requiring thoughtful answers:

- Written to `aidlc-docs/inception/` with `[Answer]:` tags
- Organized by mandatory analysis categories (12 for requirements, 10 for design, 12 for stories, 9 for units)
- Multi-round: Round 1 covers all categories, Round 2 follows up on ambiguities, Round 3 (optional) confirms remaining decisions

!!! info "Minimum Question Standards"
    INCEPTION agents have mandatory minimums to prevent shallow analysis:

    | Agent | Minimum Questions |
    |:------|:------------------|
    | Requirements Analyst | 15 (simple) / 20 (moderate) / 25 (complex) |
    | Story Writer | 10 |
    | Application Designer | 10 |
    | Units Planner | 8 |

All decisions are documented in the `aidlc-docs/` audit trail with ISO 8601 timestamps.

---

## Stage Banners (MOTD)

Every agent displays a **formatted banner** when it starts, so you always know which stage is running:

```
AI-DLC | INCEPTION Phase | Stage 3 of 7

Requirements Analysis

Agent: aidlc-requirements-analyst | Model: Opus

12-category deep questioning · Multi-round Q&A · Hybrid interactive + document
```

The banner shows:

- **Phase** (INCEPTION, CONSTRUCTION, or OPERATIONS)
- **Stage number** out of total stages in that phase
- **Agent name** and **model tier** (Opus, Sonnet, or Haiku)
- **Key capabilities** of that stage

For per-unit CONSTRUCTION stages, the banner also includes the unit name being processed (e.g., "Functional Design -- `auth-service`").

The orchestrator displays its own stage banner before delegating to each agent, ensuring banners appear even when agents are run standalone via individual commands like `/aidlc-requirements-analysis`.

---

## Parallel Unit Execution

For projects with 3 or more units, AI-DLC offers **parallel execution mode** during the Construction phase:

- **Sequential** (default) -- One unit at a time, maximum context consistency between units
- **Parallel** -- Independent units execute simultaneously in parallel groups, faster completion

!!! tip
    The Units Generation stage automatically identifies which units can run in parallel by analyzing inter-unit dependencies. Units are grouped: Group A (no dependencies, start immediately), Group B (depends on Group A), etc.

When parallel mode is active:

- Each unit's construction pipeline runs as an independent background agent
- System NFR decisions ensure consistent architectural choices across all units
- Each unit's code lives in distinct directories to avoid file conflicts
- Shared files (package.json, docker-compose.yml) are modified only in the final Build & Test phase

---

## Generated Artifacts

All documentation goes to the `aidlc-docs/` directory. Application code and operational artifacts are placed at your workspace root.

```
aidlc-docs/
  aidlc-state.md                    # Workflow state tracking
  audit.md                          # Append-only audit trail
  graph/
    dependency-graph.json           # Code dependency graph (when enabled)
    dependency-graph.md             # Mermaid visualization (when enabled)
    dependency-graph.png            # Full graph PNG export (export mode)
    community-architecture.png      # Community view PNG (export mode)
    impact-analysis.png             # Impact view PNG (export mode)
    graph-summary.md                # Graph statistics summary (all backends)
    verification-report.md          # DB verification report (neo4j/neptune)
    infra/                          # IaC files for Neptune (neptune only)
  inception/
    plans/                          # Execution plans
    reverse-engineering/            # 8 RE artifacts (brownfield)
    requirements/                   # Requirements + question files
    user-stories/                   # Stories + personas
    application-design/             # Components, services, dependencies
  construction/
    system-nfr-decisions.md         # System-level NFR (multi-unit projects)
    plans/                          # Per-unit code plans
    {unit-name}/
      functional-design/            # Business logic, rules, entities
      nfr-requirements/             # Quality attributes, tech stack
      nfr-design/                   # Patterns, logical components
      infrastructure-design/        # Service mapping
      code/                         # Code summaries
    build-and-test/                 # Build/test instructions + execution report
  operations/
    deployment-checklist.md         # Deployment steps and validation
    developer-readme.md             # Developer onboarding and setup

# Workspace root (application + operational artifacts)
.env.example                        # Environment configuration template
.github/workflows/ci.yml            # CI/CD pipeline (or .gitlab-ci.yml)
Dockerfile                          # Container image (conditional)
docker-compose.yml                  # Multi-service setup (conditional, multi-unit)
README.md                           # Project README (generated or updated)
tests/                              # Generated test files alongside application code
  integration/                      # Integration tests (multi-unit)
  e2e/                              # E2E test scaffolds (web apps, optional)
```

!!! warning
    Application code is always generated at the **workspace root**, never inside `aidlc-docs/`. The `aidlc-docs/` directory contains only documentation and workflow artifacts.
