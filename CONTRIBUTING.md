# Contributing to AI-DLC for Claude Code

Thank you for your interest in contributing! This guide will help you get started.

## How to Contribute

### Reporting Issues

- Use [GitHub Issues](https://github.com/mateon01/aidlc-for-claude/issues) to report bugs or suggest features
- Include your Claude Code version and plugin version
- Describe the expected vs actual behavior
- If possible, include the stage and command where the issue occurred

### Suggesting Enhancements

- Open an issue with the `enhancement` label
- Describe the use case and why it would be valuable
- Reference the relevant workflow stage if applicable

### Submitting Changes

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Make your changes
4. Test locally by installing the plugin from your fork
5. Commit with a clear message
6. Push and open a Pull Request

## Project Structure

```
aidlc-for-claude/
  .claude-plugin/         # Plugin metadata
    plugin.json           # Plugin definition
    marketplace.json      # Marketplace listing
  commands/               # 15 slash commands (orchestration logic)
  agents/                 # 14 specialized agents (execution logic)
  docs/                   # GitHub Pages (Jekyll)
  package.json
  LICENSE
  README.md
```

## Architecture

### Commands vs Agents

- **Commands** (`commands/`) define orchestration logic: approval gates, state management, and workflow control
- **Agents** (`agents/`) define execution logic: step-by-step protocols for the actual work

Each command delegates to a corresponding agent via the Task tool.

### Model Tiering

- **Opus agents**: Strategic reasoning (requirements, planning, architecture, design decisions)
- **Sonnet agents**: Volume work (functional design, code generation, testing)

When adding a new agent, choose the model tier based on whether the task requires deep reasoning (Opus) or efficient execution (Sonnet).

### Naming Conventions

- Commands: `aidlc-{stage-name}.md` (e.g., `aidlc-requirements-analysis.md`)
- Agents: `aidlc-{role-name}.md` (e.g., `aidlc-requirements-analyst.md`)

### File Format

Commands and agents are markdown files with specific frontmatter and structure. Refer to existing files as templates.

## Testing

### Local Plugin Testing

```bash
# Test in a new project
mkdir test-project && cd test-project
claude
# Run /plugins → Add Marketplace (local fork path) → Install → /aidlc
```

### What to Test

- Command recognition (shows in autocomplete)
- Workflow progression (stages execute in order)
- Approval gates (user is prompted before each stage)
- State management (session can be resumed)
- Artifact generation (files created in correct locations)

## Code of Conduct

- Be respectful and constructive
- Focus on the technical merits of contributions
- Help others learn and improve

## Questions?

Open an issue or start a discussion on the [GitHub repository](https://github.com/mateon01/aidlc-for-claude).

## License

By contributing, you agree that your contributions will be licensed under the Apache-2.0 License.
