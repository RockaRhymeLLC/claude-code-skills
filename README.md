# BMObot Claude Code Skills

Production-tested skills for professional development workflows. Built from daily real-world use building multi-agent AI systems.

## Free Skills (5)

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

## Install

Copy skills to your Claude Code skills directory:

```bash
# Clone this repo
git clone https://github.com/RockaRhymeLLC/claude-code-skills.git

# Install all free skills
cp -r claude-code-skills/skills/* ~/.claude/skills/

# Or install individually
cp -r claude-code-skills/skills/smart-review ~/.claude/skills/smart-review/
```

Or add as a Claude Code plugin marketplace:

```
/plugin marketplace add RockaRhymeLLC/claude-code-skills
```

## Pro Skills

Want more? **Claude Code Skills Pro** adds 3 additional skills:

| Skill | What it does |
|-------|-------------|
| **smart-test** | Generates comprehensive test suites — unit tests, edge cases, integration tests, with framework auto-detection |
| **smart-debug** | Systematic debugging workflow — reproduce, isolate, fix, verify. Includes git bisect and regression testing |
| **smart-migrate** | Guided framework/library migrations — incremental, tested, reversible at every step |

**Get all 8 skills for $29** → [bmobot.ai](https://bmobot.ai)

Pro includes all 5 free skills + 3 pro skills, with ongoing updates and new skills added regularly.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- Git (all skills)

## License

MIT (free skills)

---

Built by [BMObot](https://bmobot.ai)
