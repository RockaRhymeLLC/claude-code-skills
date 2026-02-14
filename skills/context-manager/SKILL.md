---
name: context-manager
description: Manages Claude Code context window to prevent information loss. Tracks usage, saves state before compaction, and restores seamlessly after restart. Prevents the frustrating loss of work-in-progress when context fills up. Use when working on long tasks, when context is getting full, or when the user says /ctx.
license: MIT
compatibility: Requires Claude Code CLI running in tmux
metadata:
  author: bmobot
  version: "1.0"
---

# Context Manager

Prevent information loss when your Claude Code context window fills up. Automatically tracks usage, saves state, and restores after restart.

## When to Activate

- User says `/ctx` or `/ctx save` or `/ctx check`
- User asks about context usage or remaining space
- User is working on a long task that might exhaust context
- Before starting a large task (reading many files, big implementation)

## The Problem

Claude Code has a finite context window. When it fills up:
- The system compresses earlier messages (lossy)
- You lose working state, partial progress, and nuanced context
- Multi-step tasks break halfway through
- You repeat work you already did

## Instructions

### `/ctx check` — Check Current Usage

Read the context-usage.json file (updated by Claude Code hooks):

```bash
cat .claude/state/context-usage.json 2>/dev/null
```

Report to the user:
- **Green (>60% remaining)**: Plenty of room. Work freely.
- **Yellow (40-60% remaining)**: Getting warm. Save state before starting big tasks.
- **Red (<40% remaining)**: Save state now. Consider restarting before continuing.

If no tracking file exists, guide setup (see Setup section).

### `/ctx save` — Save Current State

Capture everything needed to resume after a restart:

1. **Create state file** at `.claude/state/assistant-state.md`:

```markdown
# Assistant State

**Saved**: {ISO timestamp}
**Reason**: {why saving — manual save, low context, task checkpoint}

## Current Task
{What you're working on right now}

## What's Done
{Completed steps, with file paths and commit hashes}

## What's Next
{Remaining steps, in order}

## Key Context
{Important facts that would be lost — variable names, design decisions,
 user preferences expressed during this session, error messages that
 led to the current approach}

## Open Files
{Files currently being worked on, with relevant line ranges}
```

2. **Confirm save** — tell the user what was captured

### `/ctx restore` — Restore After Restart

At session start, check for saved state:

```bash
cat .claude/state/assistant-state.md 2>/dev/null
```

If state exists:
1. Read and internalize the saved context
2. Summarize to the user: "Picking up where we left off — [task summary]"
3. Continue from "What's Next"

### `/ctx auto` — Enable Automatic Management

Set up a hook that monitors context usage and auto-saves:

Create `.claude/hooks/context-watchdog.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": { "tool_name": "*" },
        "hooks": [
          {
            "type": "command",
            "command": "node -e \"const f='.claude/state/context-usage.json';const fs=require('fs');try{const d=JSON.parse(fs.readFileSync(f));if(d.remaining_percentage<35){fs.writeFileSync('.claude/state/context-watchdog-triggered','1');console.log('CONTEXT LOW: '+d.remaining_percentage+'% remaining. Run /ctx save now.')}}catch(e){}\""
          }
        ]
      }
    ]
  }
}
```

This checks context after every tool use and warns when it's getting low.

## State File Best Practices

### What to Save
- Current task description and progress
- File paths and line numbers being worked on
- Commit hashes of completed work
- Design decisions made during this session
- User preferences expressed (e.g., "keep it simple", "use TypeScript")
- Error messages that informed the current approach
- Open questions or blockers

### What NOT to Save
- Full file contents (they're on disk — just save the path)
- Entire conversation history (save the conclusions, not the discussion)
- Stale information from earlier in the session that was superseded

### Save Points

Good times to save:
- After completing a story or task step
- Before reading a large file or codebase exploration
- Before switching topics
- When the user takes a break
- After resolving a tricky bug (save the solution context)

## Setup

### Minimal Setup (5 minutes)

1. Create the state directory:
```bash
mkdir -p .claude/state
```

2. Use `/ctx save` and `/ctx restore` manually

### Full Setup (15 minutes)

1. Create state directory and context tracking:
```bash
mkdir -p .claude/state
echo '{}' > .claude/state/context-usage.json
```

2. Add a Claude Code hook to track context usage (requires Claude Code hooks support)

3. Run your Claude Code session inside tmux for clean restart support:
```bash
tmux new-session -s claude "claude"
```

4. Enable `/ctx auto` for automatic warnings

## Tips

- **Save early, save often**: The cost of saving is low. The cost of losing context is high.
- **Restart > clear**: `/clear` only clears history within the session. Restarting Claude Code gives a completely fresh context window.
- **Front-load reading**: Read all the files you'll need at the start of a task, before context fills up with working state.
- **Use state files as TODO lists**: "What's Next" in the state file doubles as your task list after restart.
- **Checkpoint after breakthroughs**: If you just figured out a tricky problem, save the insight immediately.
