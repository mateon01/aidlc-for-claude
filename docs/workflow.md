# Workflow Details

---

## INCEPTION Phase -- What and Why

The Inception phase establishes what needs to be built and why. It consists of 7 stages.

### Stage 1: Workspace Detection

Scans the workspace to determine if it's greenfield (new project) or brownfield (existing code). For brownfield projects, this stage also performs a **scope assessment** -- asking whether the change is simple, complex, or requires the full structured workflow. This determines the fast path routing.

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

Gathers and analyzes requirements using file-based questionnaires. Depth adapts to complexity -- simple bug fixes get minimal questions, complex systems get thorough analysis.

### Stage 4: User Stories (Conditional)

Creates user stories following INVEST criteria (Independent, Negotiable, Valuable, Estimable, Small, Testable). Includes persona definition and acceptance criteria.

### Stage 5: Workflow Planning

!!! important
    This is the critical decision stage. It determines which subsequent stages are needed.

Evaluates all gathered context and produces an execution plan. You can override any recommendation.

### Stage 6: Application Design (Conditional)

Designs component architecture, service layers, and inter-service dependencies. Produces design documents with ASCII diagrams.

### Stage 7: Units Generation (Conditional)

Decomposes the system into implementation units. Each unit becomes the scope for the Construction phase's per-unit loop.

---

## CONSTRUCTION Phase -- How

The Construction phase implements the system. For each unit, it runs a loop of stages, followed by a final build and test stage.

### Per-Unit Loop

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

    1. **Planning** (Opus) -- Creates a detailed code generation plan with file-by-file breakdown
    2. **Execution** (Sonnet) -- Generates actual code following the plan

    For brownfield projects, a git branch (`aidlc/{unit-name}`) is created before modifying existing files, providing a safe rollback point.

    After code generation, a **quality gate** runs a type/syntax check (tsc, py_compile, cargo check, go vet) and auto-fixes issues up to 2 times before presenting results.

### Build and Test

After all units are complete, generates build instructions and test plans, then **executes actual builds and tests**. The agent detects the project's build system (npm, pip, cargo, etc.), installs dependencies, runs the build, and executes tests. Failed builds are retried up to 3 times with automated fix attempts.

---

## OPERATIONS Phase (Future)

Placeholder for deployment, monitoring, and operational concerns. Will be implemented in a future release.

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
| Functional Design | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| NFR Requirements | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| NFR Design | -- | -- | :material-check: | :material-dots-horizontal: | -- |
| Infrastructure Design | -- | -- | :material-check: | :material-dots-horizontal: | :material-check: |
| Code Generation | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |
| Build and Test | :material-check: | :material-check: | :material-check: | :material-check: | :material-check: |

!!! note "Legend"
    :material-check: = Always executed | :material-dots-horizontal: = Conditional | -- = Skipped

---

## Model Strategy

AI-DLC uses three model tiers for cost-efficiency:

| Model | Role | Used For |
|:------|:-----|:---------|
| **Opus** | Strategic reasoning | Requirements analysis, architectural decisions, planning, system decomposition |
| **Sonnet** | Volume execution | Functional design, NFR analysis, code generation, testing, user stories |
| **Haiku** | Fast detection | Workspace scanning, quick project classification |
