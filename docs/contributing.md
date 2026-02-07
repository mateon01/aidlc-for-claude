# Contributing

Thank you for your interest in contributing! This guide will help you get started.

---

## How to Contribute

### Reporting Issues

Use [GitHub Issues](https://github.com/mateon01/aidlc-for-claude/issues) to report bugs or suggest features. Please include:

- Claude Code version and plugin version
- Expected vs actual behavior
- The stage and command where the issue occurred

### Suggesting Enhancements

Open an issue with the `enhancement` label. Describe the use case and reference the relevant workflow stage if applicable.

### Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test locally by installing the plugin from your fork
5. Commit with a clear message
6. Push and open a Pull Request

---

## Architecture

### Commands vs Agents

!!! note
    **Commands** define orchestration logic. **Agents** define execution logic.

| Layer | Directory | Responsibility |
|:------|:----------|:--------------|
| Commands | `commands/` | Approval gates, state management, workflow control |
| Agents | `agents/` | Step-by-step execution protocols |

Each command delegates to a corresponding agent via the Task tool.

### Model Tiering

| Tier | Model | Use For |
|:-----|:------|:--------|
| Strategic | **Opus** | Requirements, planning, architecture, design decisions |
| Execution | **Sonnet** | Functional design, code generation, testing |

!!! tip
    When adding a new agent, choose the tier based on whether the task requires deep reasoning (Opus) or efficient execution (Sonnet).

### Naming Conventions

- Commands: `aidlc-{stage-name}.md` (e.g., `aidlc-requirements-analysis.md`)
- Agents: `aidlc-{role-name}.md` (e.g., `aidlc-requirements-analyst.md`)
- Agent references in commands: `aidlc-for-claude:aidlc-{role-name}` (fully qualified with plugin prefix)

---

## Local Testing

### Install from local fork

```bash
claude plugin marketplace add /path/to/your/fork
claude plugin install aidlc-for-claude
```

### Test checklist

- [ ] Command recognition (shows in autocomplete)
- [ ] Workflow progression (stages execute in order)
- [ ] Approval gates (user is prompted before each stage)
- [ ] State management (session can be resumed)
- [ ] Artifact generation (files created in correct locations)

---

## License

By contributing, you agree that your contributions will be licensed under the [Apache-2.0 License](https://github.com/mateon01/aidlc-for-claude/blob/main/LICENSE).
