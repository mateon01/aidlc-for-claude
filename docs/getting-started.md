---
title: Getting Started
layout: default
nav_order: 2
---

# Getting Started
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Installation

### Claude Code CLI

```bash
claude plugin marketplace add https://github.com/mateon01/aidlc-for-claude
claude plugin install aidlc-for-claude
```

### Verify Installation

```bash
claude plugin list
```

You should see `aidlc-for-claude` listed as enabled.

## Quick Start

Launch Claude Code in your project directory and run:

```
/aidlc
```

The orchestrator will:

1. **Detect your workspace** -- greenfield (empty) or brownfield (existing code)
2. **Gather requirements** -- ask what you want to build
3. **Create an execution plan** -- determine which stages are needed
4. **Walk through each stage** -- with your approval at every step

## Running Individual Stages

You can run any stage independently:

```
/aidlc-requirements-analysis
/aidlc-code-generation
```

This is useful when you want to re-run a specific stage or start from a particular point in the workflow.

## Session Continuity

If your session is interrupted, simply run `/aidlc` again. The orchestrator detects the existing state file (`aidlc-docs/aidlc-state.md`) and offers to resume from where you left off.

## How Questions Work

AI-DLC never asks clarifying questions in chat. Instead, it creates markdown files with `[Answer]:` tags:

```markdown
## Question 1: Target Platform
What platform should this application target?
[Answer]:

## Question 2: Authentication Method
Which authentication method do you prefer?
[Answer]:
```

You fill in the answers in the file, and the workflow continues. This keeps all decisions documented and auditable.

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
