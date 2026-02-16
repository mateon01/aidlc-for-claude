# Workflow Details

---

## INCEPTION Phase -- What and Why

The Inception phase establishes what needs to be built and why. It consists of 7 stages.

### Stage 1: Workspace Detection

Scans the workspace to determine if it's greenfield (new project) or brownfield (existing code). For brownfield projects, this stage also performs a **scope assessment** -- asking whether the change is a simple fix, a complex modification, a new component within the existing repo, or requires the full structured workflow. This determines the fast path routing.

### Stage 2: Reverse Engineering (Brownfield Only)

For existing codebases, performs deep analysis producing 8 artifacts:

| Artifact | Content |
|:---------|:--------|
| Business Overview | Domain context and purpose |
| Architecture Map | System architecture and patterns |
| Code Structure | Directory layout and organization |
| API Inventory | Endpoints, interfaces, contracts |
| Component Inventory | Modules, services, libraries |
| Tech Stack | Languages, frameworks, dependencies |
| Dependency Graph | Internal and external dependencies |
| Code Quality | Patterns, anti-patterns, tech debt |

### Stage 3: Requirements Analysis

Gathers and analyzes requirements using a multi-round questioning system across 12 mandatory analysis categories (Business Goals, Target Users, Core Features, User Workflows, Data Model, API/Integration, Security/Compliance, Performance/Scalability, Deployment, Error Handling, Technical Constraints, Timeline). Depth adapts to complexity -- simple projects get minimum 15 questions, moderate projects 20, and complex systems 25+. Follow-up rounds resolve ambiguities and contradictions. Critical decisions (tech stack, deployment target, auth method) use interactive Q&A for immediate response.

### Stage 4: User Stories (Conditional)

Creates user stories following INVEST criteria (Independent, Negotiable, Valuable, Estimable, Small, Testable). Includes persona definition and acceptance criteria. Questions cover 12 mandatory categories including error scenarios, edge cases, accessibility/i18n, and data privacy/consent. Minimum 10 questions required.

### Stage 5: Workflow Planning

!!! important
    This is the critical decision stage. It determines which subsequent stages are needed.

Evaluates all gathered context and produces an execution plan. You can override any recommendation.

During Workflow Planning, you are offered an opt-in for **dependency graph analysis** with a multi-tier configuration flow. If you enable graph analysis, you choose a backend:

- **Neo4j Local** (recommended) -- Docker-based with Cypher queries and browser visualization at localhost:7474
- **AWS Neptune** -- Managed graph DB with IaC provisioning (CDK, Terraform, or CloudFormation) and IAM auth
- **File-based** -- Simple JSON file with no external dependencies

For Neo4j and Neptune backends, additional configuration questions cover ports, persistence, AWS region, instance class, and deployment verification level. When enabled:

- **Reverse Engineering** includes dependency graph construction from existing code
- **Code Generation** includes real-time graph updates as code is generated
- **Build & Test** includes graph-based impact analysis for test prioritization and deployment verification

The recommendation is context-aware: enabled by default for complex brownfield changes and multi-unit greenfield projects, disabled for simple changes.

### Stage 6: Application Design (Conditional)

Designs component architecture, service layers, and inter-service dependencies across 10 mandatory categories (Component Identification, Methods, Service Layer, Dependencies, Design Patterns, Scalability, Data Architecture, Security Architecture, Error Handling Strategy, API Design). Minimum 10 questions. Critical architectural choices (monolith vs microservices, DB type, communication pattern) use interactive Q&A. Produces design documents with ASCII diagrams.

### Stage 7: Units Generation (Conditional)

Decomposes the system into implementation units across 9 mandatory categories including team assignment, parallel execution feasibility, and integration points. Each unit includes parallel group metadata (dependency graph, parallel group assignment, integration contracts). For projects with 3+ units, offers parallel execution mode where independent units are processed simultaneously in the Construction phase.

---

## CONSTRUCTION Phase -- How

The Construction phase implements the system. For multi-unit projects, it begins with system-level NFR assessment, then runs a loop of stages for each unit (sequentially or in parallel), followed by final build and test stages and operations generation.

**Parallel execution** is available for projects with 3+ units. Units with no inter-dependencies are grouped and processed simultaneously. The orchestrator reads the dependency graph from Units Generation and groups units into parallel batches (Group A = no dependencies, Group B = depends on Group A, etc.). Each unit's construction pipeline runs independently with its own approval gates. Shared files are only modified in the final Build & Test phase to avoid conflicts.

### Stage 0: System NFR Assessment

For projects with 2 or more units, AI-DLC performs a one-time system-level NFR assessment before entering the per-unit loop. This establishes cross-cutting architectural decisions that apply to all units:

- **Authentication & Authorization** -- Session management, token handling, RBAC strategy
- **Observability** -- Logging standards, monitoring approach, tracing strategy
- **Error Handling** -- Error propagation patterns, retry policies, circuit breakers
- **Data Consistency** -- Transaction boundaries, eventual consistency patterns
- **API Conventions** -- Versioning strategy, pagination, error response formats

This prevents contradictory choices across units and ensures architectural coherence. Single-unit projects skip this stage and make NFR decisions during the unit's own NFR Requirements stage.

### Per-Unit Loop (Sequential or Parallel)

For each unit defined in Inception:

!!! info "Cross-unit Context"
    When starting Unit N (where N > 1), the agent receives summaries of all completed units (1 through N-1). This includes functional design summaries, tech stack decisions, shared patterns, and domain entities -- ensuring consistency across the entire system.

=== "Step 1: Functional Design"

    Business logic, domain models, rules, and entities specific to this unit. Reads prior units' artifacts to maintain consistent domain models and conventions.

=== "Step 2: NFR Requirements"

    Quality attributes (performance, security, reliability) and technology stack decisions. Maintains consistency with technologies chosen by prior units unless there is a compelling reason to diverge.

=== "Step 3: NFR Design"

    Patterns and logical components that satisfy the non-functional requirements.

=== "Step 4: Infrastructure Design"

    Maps logical components to infrastructure services (databases, caches, queues, etc.).

=== "Step 5: Code Generation"

    Two-part stage:

    1. **Planning** (Opus) -- Creates a detailed code generation plan with file-by-file breakdown and **mandatory test plan** per module (test file paths, test cases, assertions, coverage targets)
    2. **Execution** (Sonnet) -- Generates actual application code **and test files** following the plan

    For brownfield projects, a git branch (`aidlc/{unit-name}`) is created before modifying existing files, providing a safe rollback point.

    After code generation, a **multi-layer quality gate** runs:

    - Type/syntax check (tsc, py_compile, cargo check, go vet)
    - Lint check (ESLint, Ruff, Clippy, golangci-lint) if linter is configured
    - Unit test execution on generated tests
    - Optional design conformance check (verifies code matches design artifacts)

    Issues are auto-fixed up to 3 times before presenting results.

### Build and Test

When dependency graph analysis is enabled (`graphEnabled: true`), an **impact analysis** step runs before test execution. It reads the dependency graph, identifies affected modules via BFS/DFS traversal, maps them to test files, and constructs a prioritized test execution plan:

- **Priority 1** (direct changes): Tests for directly changed files, run first with `--bail`
- **Priority 2** (1-hop dependents): Tests for direct dependents, run second
- **Priority 3** (full suite): All tests to catch transitive effects, run last
- Modules with no matching test files are flagged as "untested dependencies"

Test execution follows priority order. If P1 fails, P2 and P3 still run for completeness. If P1+P2 pass but P3 fails, the report highlights the unexpected transitive effect.

After all units are complete, generates build instructions and test plans, then **executes actual builds and tests**. The agent detects the project's build system (npm, pip, cargo, etc.), installs dependencies, runs a **dependency security scan**, runs the build, executes tests **with coverage tracking**, and generates **integration tests** for multi-unit projects. Failed builds are retried up to 3 times with automated fix attempts. For web applications, an optional **E2E test scaffold** (Playwright/Cypress) can be generated.

### Operations

Generates operational artifacts at the workspace root and in aidlc-docs:

1. **Deployment Checklist** -- Step-by-step deployment guide with environment validation, dependency checks, configuration steps, and smoke test procedures
2. **Developer README** -- Onboarding guide with setup instructions, development workflow, testing procedures, and troubleshooting tips
3. **`.env.example`** -- Environment configuration template with all required variables extracted from design artifacts and code
4. **CI/CD Pipeline** -- GitHub Actions or GitLab CI workflow with install, lint, build, test, and security audit steps
5. **Dockerfile** (conditional) -- Multi-stage production Dockerfile optimized for the detected stack
6. **Docker Compose** (conditional, multi-unit) -- Local development stack with all services and dependencies
7. **Root README.md** -- Project README at workspace root with quick start and architecture overview
8. **Monitoring config** (conditional) -- Structured logging configuration and health check setup

---

## OPERATIONS Phase

The Operations phase ensures smooth deployment and developer onboarding. It produces both documentation artifacts (in `aidlc-docs/operations/`) and executable artifacts (at the workspace root) based on the completed construction artifacts. Executable artifacts include CI/CD pipelines, Dockerfiles, environment templates, and README files -- making the generated project immediately deployable and developer-ready.

### PR Review Workflow

The Operations phase can optionally generate an AI-powered PR review workflow (`.github/workflows/pr-review.yml`) that automatically reviews pull requests. You choose between Claude API or OpenAI API as the AI backend. The workflow posts a single review comment on each PR with findings organized by category.

### Issue and PR Templates

The Operations phase also generates GitHub issue templates (`.github/ISSUE_TEMPLATE/`) and a PR template (`.github/PULL_REQUEST_TEMPLATE.md`) customized to the project's components and conventions. These standardize how contributors report bugs, request features, and describe pull requests.

---

## Standalone Utilities

These commands can be run independently of the three-phase workflow at any time:

| Command | Description |
|:--------|:------------|
| `/aidlc-review-pr` | Analyze PR diffs or local changes for code quality, security, performance, and consistency |
| `/aidlc-ci-setup` | Generate CI/CD pipelines, PR review workflows, and issue/PR templates for any project |
| `/aidlc-graph` | Build, update, visualize, or analyze code dependency graphs |

The **PR review** utility performs a 6-category analysis (correctness, security, performance, consistency, testing, documentation) and presents a structured report with per-file findings and a verdict.

The **CI setup** utility detects your project's tech stack automatically and generates selected infrastructure files (CI/CD pipeline, PR review workflow, issue templates, PR template). It also provides branch protection recommendations.

The **graph analysis** utility supports seven modes: build (full static analysis), update (incremental), visualize (Mermaid diagram), impact analysis (affected module detection with test prioritization), search (GraphRAG semantic code retrieval), verify (9-point DB health check), and teardown (stop/remove graph DB). It supports three backends: File-based JSON, Neo4j (local Docker with Cypher), and AWS Neptune (managed graph DB with IaC provisioning). When GraphRAG is enabled (`graphRAGEnabled: true`), module summaries (purpose, keywords, architectural layer) and community structure are generated alongside the dependency graph -- no external embedding models required. Neo4j uses full-text indexes for search; File backend uses keyword matching. E2E verified with Neo4j backend on a 15-module TypeScript project -- 15 nodes, 41 edges loaded, all 9 verification checks passed, hub analysis identified critical modules, and impact analysis correctly traced direct and transitive dependencies.

All graph node IDs follow a strict normalization convention for consistency across units and queries: `u-{NN}` for units (lowercase, zero-padded), `mod-{kebab-name}` for modules, `file-{kebab-name}` for files, `comm-{kebab-name}` for communities, and `summary-{parent-id}` for summaries. IDs are always lowercase, hyphen-separated, and type-prefixed.

!!! info "Automatic Graph Maintenance"
    When using the full AI-DLC workflow with `graphEnabled: true`, the dependency graph is automatically maintained throughout the pipeline. Reverse Engineering builds the initial graph (brownfield), Code Generation updates it incrementally per unit with post-update integrity verification, and Build & Test uses impact analysis for prioritized test execution. The orchestrator manages graph DB lifecycle -- initialization before the first graph operation, health checks between stages, serialization of updates during parallel execution, and a teardown offer at workflow completion. All graph operations are non-blocking: failures log warnings and continue the workflow. You can retry any graph operation later with `/aidlc-graph`.

!!! warning "Neptune CloudFormation Cleanup"
    When tearing down Neptune via CloudFormation, stack deletion may be blocked by orphaned resources: VPC Endpoint ENIs (persist in private subnets after cluster deletion) and GuardDuty-managed security groups (auto-created if GuardDuty is enabled). Delete these resources manually before retrying `aws cloudformation delete-stack`. See the graph-analyzer teardown instructions for detailed cleanup commands.

---

## Stage Execution Matrix

| Stage | Simple Bug Fix | Fast Path | New Feature | Brownfield | Infra Change |
|:------|:-:|:-:|:-:|:-:|:-:|
| Workspace Detection | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |
| Reverse Engineering | -- | :material-check: | -- | :material-check: | -- |
| Requirements Analysis | :material-check: | -- | :material-check: | :material-check: | :material-check: |
| User Stories | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| Workflow Planning | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |
| Application Design | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| Units Generation | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| System NFR Assessment* | -- | -- | :material-dots-horizontal: | :material-dots-horizontal: | -- |
| Functional Design | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| NFR Requirements | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| NFR Design | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| Infrastructure Design | -- | -- | :material-check: | :material-dots-horizontal: | :material-check: |
| Code Generation | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |
| Build and Test | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |
| Operations | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |

!!! note "Legend"
    :material-check: = Always executed | :material-dots-horizontal: = Conditional | -- = Skipped

    \* System NFR Assessment only executes when 2+ units are generated

---

## Stage Banners (MOTD)

Every agent displays a formatted banner when it starts, showing the current phase, stage number, agent name, model tier, and key capabilities. This provides clear visibility into which stage is running and what to expect. The orchestrator also displays a stage banner before each delegation.

For per-unit CONSTRUCTION stages, the banner includes the unit name (e.g., "Functional Design -- `auth-service`"). This ensures clarity even when multiple units are being processed sequentially or in parallel.

---

## Model Strategy

AI-DLC uses three model tiers for cost-efficiency:

| Model | Role | Used For |
|:------|:-----|:---------|
| **Opus** | Strategic reasoning | Requirements analysis, architectural decisions, planning, system decomposition |
| **Sonnet** | Volume execution | Functional design, NFR analysis, code generation, testing, user stories |
| **Haiku** | Fast detection | Workspace scanning, quick project classification |
