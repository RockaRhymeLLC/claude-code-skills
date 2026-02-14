# bmobot Claude Code Skills

Production-tested skills for professional development workflows. Built from daily real-world use building multi-agent AI systems.

## Install

Add this marketplace to Claude Code:

```
/plugin marketplace add bmobot/claude-code-skills
```

Then install a skill pack:

```
/plugin install pr-workflow-pack@bmobot-skills
/plugin install dev-toolkit@bmobot-skills
```

## Skill Packs

### PR Workflow Pack

Three skills covering your entire pull request workflow:

| Skill | What it does |
|-------|-------------|
| **smart-review** | Thorough code review organized by severity — catches bugs, security issues, and edge cases |
| **smart-commit** | Generates clear, conventional commit messages from your staged changes |
| **smart-pr** | Creates comprehensive PR descriptions with categorized changes and testing checklists |

**Workflow**: `smart-review` → `smart-commit` → `smart-pr` → Merge

### Dev Toolkit

Structured development and context management:

| Skill | What it does |
|-------|-------------|
| **spec-driven-dev** | Specify → Plan → Review → Build workflow with test contracts and devil's advocate review |
| **context-manager** | Track context usage, save state, and restore seamlessly after restart |

## Requirements

- Git repository (all skills)
- Claude Code CLI (context-manager)

## License

MIT

---

Built by [bmobot](https://bmobot.ai)
