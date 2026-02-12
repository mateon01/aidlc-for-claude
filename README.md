# AI-DLC for Claude Code

A Claude Code plugin that implements the **AI-DLC (AI-Driven Development Life Cycle)** methodology -- a structured, adaptive software development workflow that guides AI assistants through disciplined three-phase development.

AI-DLC intelligently adapts to your project: simple changes execute only essential stages, while complex projects receive comprehensive treatment with full safeguards.

**[Documentation](https://mateon01.github.io/aidlc-for-claude)** | **[Apache-2.0 License](LICENSE)**

## Installation

In the Claude Code chat, run these two commands:

```
/plugin marketplace add mateon01/aidlc-for-claude
/plugin install aidlc-for-claude
```

## Quick Start

```
/aidlc
```

That's it. The orchestrator detects your workspace (greenfield or brownfield), gathers requirements through interactive Q&A, and walks through each phase with you. Every stage requires your approval before proceeding.

To run a specific stage independently:

```
/aidlc-requirements-analysis
/aidlc-code-generation
```

## How It Works

AI-DLC has three phases. Each phase contains stages that may execute conditionally based on your project's needs.

```
+-----------------------------------------------------------------------+
|                     INCEPTION (WHAT and WHY)                          |
|                                                                       |
|  Workspace Detection --> [Scope Assessment (brownfield)]              |
|  --> Reverse Engineering --> Requirements Analysis                    |
|  --> User Stories --> Workflow Planning --> Application Design         |
|  --> Units Generation                                                 |
+-----------------------------------+-----------------------------------+
                                    |
                                    v
+-----------------------------------------------------------------------+
|                     CONSTRUCTION (HOW)                                |
|                                                                       |
|  System NFR Assessment -->                                            |
|  FOR each unit (sequential or parallel):                              |
|    Functional Design --> NFR Requirements --> NFR Design               |
|    --> Infrastructure Design --> Code Generation                      |
|                                                                       |
|  Build and Test (after all units) + security scan + coverage          |
|  Operations (CI/CD, PR Review, Dockerfile, .env, README, deploy)     |
+-----------------------------------+-----------------------------------+
                                    |
                                    v
+-----------------------------------------------------------------------+
|                     OPERATIONS                                        |
+-----------------------------------------------------------------------+
```

### Stage Execution

Not every stage runs every time. After Workflow Planning, an execution plan determines which stages are needed:

| Condition | Stages Executed |
|-----------|----------------|
| Simple bug fix | Workspace Detection, Requirements, Workflow Planning, Code Generation, Build and Test |
| Simple brownfield (fast path) | Workspace Detection, Reverse Engineering, Workflow Planning (minimal), Code Generation, Build and Test |
| New feature (greenfield) | All INCEPTION + all CONSTRUCTION stages |
| Brownfield modification | Adds Reverse Engineering, adapts scope based on existing codebase |
| Infrastructure-only change | Skips User Stories, Functional Design; includes Infrastructure Design |

You can override any recommendation at the Workflow Planning approval gate.

## Commands

| Command | Phase | Description |
|---------|-------|-------------|
| `/aidlc` | - | Main orchestrator (start here) |
| `/aidlc-workspace-detection` | INCEPTION 1 | Scan workspace, detect greenfield/brownfield |
| `/aidlc-reverse-engineering` | INCEPTION 2 | Analyze existing codebase (brownfield) |
| `/aidlc-requirements-analysis` | INCEPTION 3 | Gather and analyze requirements |
| `/aidlc-user-stories` | INCEPTION 4 | Create user stories with INVEST criteria |
| `/aidlc-workflow-planning` | INCEPTION 5 | Determine execution plan |
| `/aidlc-application-design` | INCEPTION 6 | Component and service layer design |
| `/aidlc-units-generation` | INCEPTION 7 | Decompose system into units |
| `/aidlc-system-nfr` | CONSTRUCTION 0 | System-level NFR decisions (multi-unit projects) |
| `/aidlc-functional-design` | CONSTRUCTION 1 | Business logic design (per-unit) |
| `/aidlc-nfr-requirements` | CONSTRUCTION 2 | Non-functional requirements (per-unit) |
| `/aidlc-nfr-design` | CONSTRUCTION 3 | NFR pattern design (per-unit) |
| `/aidlc-infrastructure-design` | CONSTRUCTION 4 | Infrastructure mapping (per-unit) |
| `/aidlc-code-generation` | CONSTRUCTION 5 | Code generation (per-unit) |
| `/aidlc-build-and-test` | CONSTRUCTION 6 | Build, test (coverage), security scan, integration/E2E tests |
| `/aidlc-operations` | OPERATIONS | CI/CD, Dockerfile, .env.example, README, deployment checklist |
| `/aidlc-review-pr` | UTILITY | Analyze PR diffs for code quality, security, and consistency |
| `/aidlc-ci-setup` | UTILITY | Generate CI/CD pipelines, PR review workflows, and issue/PR templates |
| `/aidlc-graph` | UTILITY | Build, update, and visualize code dependency graphs |

## Agents

Each command delegates to a specialized agent via the Task tool. Agents use the fully qualified `aidlc-for-claude:` prefix and are tiered by model for cost-efficiency:

### Opus (Strategic Reasoning)

| Agent | Purpose |
|-------|---------|
| `aidlc-for-claude:aidlc-reverse-engineer` | Deep codebase analysis for brownfield projects |
| `aidlc-for-claude:aidlc-requirements-analyst` | Requirements gathering with adaptive depth |
| `aidlc-for-claude:aidlc-story-writer` | User story creation with INVEST criteria |
| `aidlc-for-claude:aidlc-workflow-planner` | Execution planning and stage determination |
| `aidlc-for-claude:aidlc-application-designer` | Component and service layer design |
| `aidlc-for-claude:aidlc-units-planner` | System decomposition into units |
| `aidlc-for-claude:aidlc-code-planner` | Code generation plan with mandatory test plans per module |

### Sonnet (Volume Work)

| Agent | Purpose |
|-------|---------|
| `aidlc-for-claude:aidlc-functional-designer` | Business logic and domain model design |
| `aidlc-for-claude:aidlc-nfr-analyst` | Non-functional requirements assessment |
| `aidlc-for-claude:aidlc-system-nfr-analyst` | System-level NFR decisions for multi-unit projects |
| `aidlc-for-claude:aidlc-nfr-designer` | NFR pattern and component design |
| `aidlc-for-claude:aidlc-infra-designer` | Infrastructure service mapping |
| `aidlc-for-claude:aidlc-code-generator` | Code + test generation with multi-layer quality gate |
| `aidlc-for-claude:aidlc-build-test-engineer` | Build, test (coverage), security scan, integration/E2E scaffolding |
| `aidlc-for-claude:aidlc-ops-generator` | CI/CD, Dockerfile, Docker Compose, .env.example, README, deployment checklist |
| `aidlc-for-claude:aidlc-pr-reviewer` | PR diff analysis for code quality, security, performance, and consistency |
| `aidlc-for-claude:aidlc-ci-setup-engineer` | CI/CD pipeline, PR review workflow, and issue/PR template generation |
| `aidlc-for-claude:aidlc-graph-analyzer` | Code dependency graph with multi-backend support (File/Neo4j/Neptune), impact analysis, and visualization |

### Haiku (Fast Detection)

| Agent | Purpose |
|-------|---------|
| `aidlc-for-claude:aidlc-workspace-analyst` | Workspace scanning and project type detection |

**Model strategy:** Opus handles stages requiring deep reasoning (requirements analysis, architectural decisions, planning). Sonnet handles volume work (design documents, code generation, testing, operations). Haiku handles fast detection (workspace scanning, project classification).

Total: **19 specialized agents** across 3 model tiers.

## Key Conventions

**Deep questioning** -- INCEPTION stages ask 15-30 practical questions across mandatory analysis categories (12 for requirements, 10 for application design, 12 for stories, 9 for units) with multi-round md file Q&A. Each category requires at least 1 question, and agents have mandatory minimums to prevent shallow analysis.

**Interactive Q&A** -- Simple preference questions use Claude Code's interactive question UI (AskUserQuestion) with clickable options. Complex questions with many items use document-based questionnaires. All decisions are documented in the audit trail.

**Brownfield fast path** -- For existing codebases, a scope assessment after workspace detection lets you choose between four options: simple change (fast path to code generation), complex change (streamlined path), new component within existing repo, or full structured workflow.

**Prerequisite validation** -- Construction agents verify required input files exist before proceeding, failing fast with clear errors if a prior stage was skipped.

**System-level NFR** -- For multi-unit projects, cross-cutting NFR decisions (authentication, observability, error handling) are established once before the per-unit loop, preventing contradictory choices.

**Approval gates** are required at each stage. You review the generated artifacts and choose to approve, request changes, or add/remove stages.

**Audit trail** (`aidlc-docs/audit.md`) captures every user input with ISO 8601 timestamps. It is append-only and never overwritten.

**Session continuity** is handled via `aidlc-docs/aidlc-state.md`. Re-running `/aidlc` detects existing state and offers to resume.

**Error recovery** -- Failed stages can be retried, skipped, or aborted. State is preserved for session continuity. Build failures are retried up to 3 times with automated fix attempts.

**Stage banners (MOTD)** -- Every agent displays a formatted banner on start showing phase, stage number, agent name, model, and key capabilities. The orchestrator also shows a stage banner before each delegation, so you always know which stage is running and what to expect.

**Parallel unit execution** -- CONSTRUCTION supports concurrent unit processing for projects with 3 or more units. Units with no inter-dependencies execute simultaneously in parallel groups, reducing total build time. File ownership ensures no conflicts between parallel units. Shared files are modified only in the final Build & Test phase.

**Batch approval** -- Optionally auto-approve construction design stages (Functional Design through Infrastructure Design) to reduce review overhead on large projects. Code Generation and Build & Test still require explicit review.

**Git safety** -- Brownfield code generation creates a `aidlc/{unit-name}` git branch before modifying existing files, providing a safe rollback point.

**Code quality gate** -- Before presenting generated code, the agent runs a multi-layer quality check: type/syntax check (tsc, py_compile, cargo check, go vet), lint check (ESLint, Ruff, Clippy if configured), and unit test execution. Auto-fixes issues up to 3 times.

**Test-first code generation** -- Code generation plans include mandatory test plans per module with test file paths, test cases, and coverage targets. Test files are generated alongside application code as first-class artifacts.

**Dependency security scanning** -- Build and Test stage runs `npm audit` / `pip audit` / `cargo audit` to catch known vulnerabilities in dependencies. Results are informational (not gating).

**Test coverage tracking** -- Test execution includes coverage flags (--coverage, --cov) when available. Coverage percentage is reported in the execution report.

**Integration test scaffolding** -- For multi-unit projects, executable integration test files are generated to verify cross-unit interfaces, API contracts, and shared data models.

**E2E test scaffolding** -- For web applications, optional Playwright or Cypress test scaffolds with basic smoke tests are generated based on data-testid attributes in UI components.

**Operations artifacts** -- The Operations phase generates executable artifacts beyond documentation: CI/CD pipeline, Dockerfile, Docker Compose, .env.example, root README.md, and monitoring configuration.

**Design conformance check** -- Optional verification that generated code matches functional design artifacts (domain entities, API endpoints, business rules).

**Cross-unit consistency** -- When building multi-unit systems, each unit receives summaries of all previously completed units to maintain consistent domain models, tech stack choices, and conventions.

**PR review** -- The `/aidlc-review-pr` standalone utility analyzes PR diffs or local changes across 6 categories (correctness, security, performance, consistency, testing, documentation). It can review GitHub PRs, local changes, or branch diffs independently of the three-phase workflow.

**CI setup** -- The `/aidlc-ci-setup` standalone utility generates CI/CD infrastructure for any project. It detects the tech stack automatically and generates CI/CD pipelines, AI-powered PR review workflows, issue templates, and PR templates -- no prior AI-DLC stages required.

**Dependency graph** -- Optional graph-based code dependency analysis with multi-backend support. During Workflow Planning, choose from three backends: **Neo4j Local** (Docker-based with Cypher queries and browser visualization), **AWS Neptune** (managed graph DB with IaC provisioning via CDK/Terraform/CloudFormation), or **File-based** (simple JSON, no dependencies). Deployment verification (9 checks: connection, schema, node/edge counts, orphan/duplicate edge detection, hub analysis, circular dependency detection, impact analysis) ensures graph DB health after setup. The standalone `/aidlc-graph` utility can build, update, visualize, verify, or tear down the graph at any time. When using the full workflow, the graph is automatically maintained: Reverse Engineering builds the initial graph (brownfield), Code Generation updates it incrementally per unit with integrity verification, and Build & Test uses impact analysis for prioritized test execution (direct dependencies first, then 1-hop, then full suite). The orchestrator manages graph DB lifecycle (initialization, health checks, parallel coordination, teardown). Graph operations are non-blocking -- failures log warnings and continue the workflow. E2E verified with Neo4j backend on a 15-module TypeScript project (15 nodes, 41 edges, all checks passed).

**Adaptive depth** means all defined artifacts for a stage are created, but the detail level adapts to problem complexity. A simple bug fix gets concise artifacts; a complex system gets comprehensive treatment.

**ASCII diagrams** use only `+`, `-`, `|`, `^`, `v`, `<`, `>` characters. No Unicode box-drawing characters.

## Artifacts

All documentation is generated in the `aidlc-docs/` directory. Application code is always placed in the workspace root.

```
aidlc-docs/
  aidlc-state.md                    # Workflow state tracking
  audit.md                          # Append-only audit trail
  graph/
    dependency-graph.json           # Code dependency graph (when enabled)
    dependency-graph.md             # Mermaid visualization (when enabled)
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
      code/                         # Code summaries (code itself at workspace root)
    build-and-test/                 # Build and test instructions + execution report
  operations/
    deployment-checklist.md         # Deployment steps and validation
    developer-readme.md             # Developer onboarding and setup

# Workspace root (application + operational artifacts)
.env.example                        # Environment configuration template
.github/workflows/ci.yml            # CI/CD pipeline (or .gitlab-ci.yml)
.github/workflows/pr-review.yml     # AI-powered PR review (conditional)
.github/ISSUE_TEMPLATE/             # Issue form templates (feature, bug)
.github/PULL_REQUEST_TEMPLATE.md    # PR template
Dockerfile                          # Container image (conditional)
docker-compose.yml                  # Multi-service setup (conditional, multi-unit)
README.md                           # Project README (generated or updated)
tests/                              # Generated test files alongside application code
  integration/                      # Integration tests (multi-unit)
  e2e/                              # E2E test scaffolds (web apps, optional)
```

## Plugin Structure

```
aidlc-for-claude/
  .claude-plugin/
    plugin.json                     # Plugin metadata
    marketplace.json                # Marketplace listing
  commands/                         # 19 slash commands
    aidlc.md                        # Entry point orchestrator
    aidlc-workspace-detection.md
    aidlc-reverse-engineering.md
    aidlc-requirements-analysis.md
    aidlc-user-stories.md
    aidlc-workflow-planning.md
    aidlc-application-design.md
    aidlc-units-generation.md
    aidlc-system-nfr.md
    aidlc-functional-design.md
    aidlc-nfr-requirements.md
    aidlc-nfr-design.md
    aidlc-infrastructure-design.md
    aidlc-code-generation.md
    aidlc-build-and-test.md
    aidlc-operations.md
    aidlc-review-pr.md
    aidlc-ci-setup.md
    aidlc-graph.md
  agents/                           # 19 specialized agents
    aidlc-workspace-analyst.md
    aidlc-reverse-engineer.md
    aidlc-requirements-analyst.md
    aidlc-story-writer.md
    aidlc-workflow-planner.md
    aidlc-application-designer.md
    aidlc-units-planner.md
    aidlc-system-nfr-analyst.md
    aidlc-functional-designer.md
    aidlc-nfr-analyst.md
    aidlc-nfr-designer.md
    aidlc-infra-designer.md
    aidlc-code-planner.md
    aidlc-code-generator.md
    aidlc-build-test-engineer.md
    aidlc-ops-generator.md
    aidlc-pr-reviewer.md
    aidlc-ci-setup-engineer.md
    aidlc-graph-analyzer.md
  docs/                             # GitHub Pages (Material for MkDocs)
  .github/
    workflows/
      deploy-docs.yml               # GitHub Pages deployment
      pr-review.yml                 # AI-powered PR review
    ISSUE_TEMPLATE/
      feature-request.yml           # Feature request form
      bug-report.yml                # Bug report form
      config.yml                    # Template config
    PULL_REQUEST_TEMPLATE.md        # PR template
  .gitignore
  package.json
  LICENSE
  CONTRIBUTING.md
  README.md
```

## Origin

AI-DLC was originally developed as an AI-driven software development methodology by AWS. This plugin adapts the methodology for Claude Code's native command/agent system:

- Kiro's rule-loading mechanism becomes self-contained agent markdown files
- Kiro's file-based approvals become interactive approval gates
- Single-AI execution becomes multi-agent delegation with model tiering (Opus/Sonnet/Haiku)

### References

- [AI-DLC Methodology Blog (AWS Tech Blog)](https://aws.amazon.com/ko/blogs/tech/ai-driven-development-life-cycle/) - Original methodology introduction
- [AI-DLC Interactive Demo](https://prod.d13rzhkk8cj2z0.amplifyapp.com/) - Live demo of the AI-DLC workflow
- [AI-DLC Workflows for Kiro (GitHub)](https://github.com/awslabs/aidlc-workflows) - Original Kiro steering files

## License

Apache-2.0
