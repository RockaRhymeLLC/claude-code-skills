# BMObot Claude Code Skills

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-7C3AED)](https://code.claude.com/docs/en/plugins)
[![Skills](https://img.shields.io/badge/Skills-6%20Free-00D4AA)](skills/)

Production-tested skills for professional development workflows. Built from daily real-world use building multi-agent AI systems.

## Install

**As a plugin** (recommended):

```bash
claude /plugin install RockaRhymeLLC/claude-code-skills
```

**Or copy skills directly:**

```bash
git clone https://github.com/RockaRhymeLLC/claude-code-skills.git
cp -r claude-code-skills/skills/* ~/.claude/skills/
```

## Skills

### smart-review

Thorough code review organized by severity — catches bugs, security issues, and edge cases that slip past manual review.

<details>
<summary>Example output</summary>

```
## Critical (1)

### SQL Injection in user search
src/routes/users.ts:42

The `searchQuery` parameter is interpolated directly into the SQL string.
An attacker could pass `'; DROP TABLE users; --` to destroy data.

Fix: Use parameterized queries.
- const results = db.query(`SELECT * FROM users WHERE name = '${searchQuery}'`);
+ const results = db.query('SELECT * FROM users WHERE name = $1', [searchQuery]);

## Warning (2)

### Missing null check on API response
src/services/payment.ts:88

`response.data.transaction` is accessed without checking if `response.data`
exists. If the payment API returns an error shape, this throws TypeError.

### Unbounded array growth in event listener
src/core/events.ts:23

Events are pushed to `this.history` but never pruned. In long-running
processes, this causes memory growth proportional to uptime.

## Suggestion (1)

### Consider extracting validation logic
src/routes/auth.ts:15-45

The email/password validation block is 30 lines and duplicated in the
registration endpoint. Extract to a shared `validateCredentials()` helper.
```

</details>

### smart-commit

Generates clear, conventional commit messages from staged changes. Analyzes diffs to explain *why*, not just *what*.

<details>
<summary>Example output</summary>

```
feat(auth): add OAuth2 login with Google provider

- Add Google OAuth strategy with PKCE flow
- Create session management middleware with secure cookie config
- Add user profile sync on first login
- Include rate limiting on auth endpoints (10 req/min)

Breaking change: SESSION_SECRET env var is now required
```

</details>

### smart-pr

Creates comprehensive PR descriptions with categorized changes, testing notes, and reviewer guidance.

<details>
<summary>Example output</summary>

```markdown
## Summary
Add Google OAuth2 as a login option alongside existing email/password auth.
Users can now sign in with their Google account in one click.

## Changes

**New Features**
- Google OAuth2 login flow with PKCE
- Automatic profile sync (name, avatar) on first Google login
- "Continue with Google" button on login page

**Infrastructure**
- Add `passport-google-oauth20` dependency
- New session middleware with secure cookie defaults
- Rate limiting on all auth endpoints

## Test Plan
- [ ] Google login flow works end-to-end
- [ ] Existing email/password login still works
- [ ] Session persists across page reloads
- [ ] Rate limiting triggers after 10 requests
- [ ] Profile sync pulls correct Google data
```

</details>

### spec-driven-dev

Full Specify, Plan, Review, Build workflow with test contracts and devil's advocate review. Prevents scope creep and ensures nothing ships without passing tests.

<details>
<summary>How it works</summary>

```
/bmobot-skills:spec-dev auth-system

1. SPECIFY  -> Creates detailed spec with requirements and constraints
2. PLAN     -> Breaks spec into stories with test contracts
3. REVIEW   -> Devil's advocate agent challenges the plan
4. BUILD    -> Implements stories one-by-one, running tests after each

Output: specs/auth-system.md, plans/auth-system.md, working code
```

</details>

### context-manager

Track context usage, save state, and restore seamlessly after restart. Never lose work when your context window fills up.

### smart-changelog

Generate professional changelogs and release notes from git history. Auto-categorizes commits, suggests semantic versions, and outputs clean [Keep a Changelog](https://keepachangelog.com/) format.

<details>
<summary>Example output</summary>

```markdown
## [1.4.0] - 2026-02-14

### Added
- Rate limiting for API endpoints (#123)
- Export to CSV from dashboard
- Dark mode support

### Fixed
- Login redirect loop on Safari (#456)
- Memory leak in WebSocket handler

### Performance
- 40% faster search with new index strategy
```

</details>

## Full Workflow

The skills work together as a complete development pipeline:

```
Code changes -> smart-review -> smart-commit -> smart-pr -> Merge -> smart-changelog -> Ship
                    |               |              |                        |
                Fix issues    Clean history   Clear PR desc         Release notes
```

## PR Workflow

The three PR skills work together as a complete workflow:

```
Code changes -> smart-review -> smart-commit -> smart-pr -> Merge
                    |               |              |
                Fix issues    Clean history   Clear PR description
```

## Pro Skills

**[Claude Code Skills Pro](https://bmobot.ai)** adds 3 additional skills for $29:

| Skill | What it does |
|-------|-------------|
| **smart-test** | Generate comprehensive test suites with framework auto-detection |
| **smart-debug** | Systematic debugging: reproduce, isolate, fix, verify with git bisect |
| **smart-migrate** | Guided framework migrations — incremental, tested, reversible |

All 9 skills (6 free + 3 pro) with ongoing updates.

## Requirements

- [Claude Code](https://code.claude.com) CLI (v1.0.33+)
- Git

## License

MIT

---

Built by [BMObot](https://bmobot.ai) — AI agents with human standards.
