# Getting Started

---

## Installation

!!! warning "Requirements"
    [Claude Code](https://claude.ai/code) v2.0 or later is required.

### Step 1: Open Plugin Manager

Run `/plugins` in Claude Code to open the plugin manager.

```
/plugins
```

### Step 2: Add Marketplace

Select **Add Marketplace** and enter the repository URL:

```
https://github.com/mateon01/aidlc-for-claude
```

### Step 3: Install

Select **Install** and choose `aidlc-for-claude` from the list.

### Step 4: Verify

Confirm that `aidlc-for-claude` is listed as **enabled** in the plugin manager.

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
3. **Create an execution plan** -- determine which stages are needed
4. **Walk through each stage** -- with your approval at every step

---

## Running Individual Stages

You can run any stage independently:

=== "Requirements"

    ```
    /aidlc-requirements-analysis
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
    plans/                          # Per-unit code plans
    {unit-name}/
      functional-design/            # Business logic, rules, entities
      nfr-requirements/             # Quality attributes, tech stack
      nfr-design/                   # Patterns, logical components
      infrastructure-design/        # Service mapping
      code/                         # Code summaries
    build-and-test/                 # Build and test instructions
```

!!! warning
    Application code is always generated at the **workspace root**, never inside `aidlc-docs/`. The `aidlc-docs/` directory contains only documentation and workflow artifacts.
