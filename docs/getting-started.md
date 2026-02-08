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

## Session Continuity

If your session is interrupted, simply run `/aidlc` again. The orchestrator detects the existing state file (`aidlc-docs/aidlc-state.md`) and offers to resume from where you left off.

---

## How Questions Work

AI-DLC uses Claude Code's interactive question UI to gather your preferences. Questions appear as clickable multiple-choice options directly in the terminal:

- **Tech stack**, **database**, **authentication**, and **scope** decisions
- Each question offers recommended defaults you can accept with one click
- You can also type custom responses

All decisions are documented in the `aidlc-docs/` audit trail with ISO 8601 timestamps.

---

## Generated Artifacts

All documentation goes to the `aidlc-docs/` directory. Application code is always placed at your workspace root.

```
aidlc-docs/
  aidlc-state.md                    # Workflow state tracking
  audit.md                          # Append-only audit trail
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
    build-and-test/                 # Build and test instructions
  operations/
    deployment-checklist.md         # Deployment steps and validation
    developer-readme.md             # Developer onboarding and setup
```

!!! warning
    Application code is always generated at the **workspace root**, never inside `aidlc-docs/`. The `aidlc-docs/` directory contains only documentation and workflow artifacts.
