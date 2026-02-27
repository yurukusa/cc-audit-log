# cc-audit-log

See what your Claude Code actually did. Human-readable audit trail from session transcripts.

```
npx cc-audit-log
```

## What it shows

- Files created and modified
- Bash commands executed
- Git commits and pushes
- Subagent spawns
- Risk flags (force pushes, recursive deletes, sudo, etc.)
- Timeline of key actions with timestamps

## Sample output

```
  Claude Code Audit Log v1.0
  ═══════════════════════════════════════
  Scanning: ~/.claude/projects/

  ▸ Session: 2026-02-27 11:47 → 15:44 (3h 56m)
    Project: nursery-shift  |  7.1MB transcript

  ▸ Summary
    Tool calls:     201
    Files created:  4
    Files modified: 6
    Files read:     18
    Bash commands:  44
    Git commits:    1

  ▸ Key Actions
    11:48  T Spawned agent: Explore nursery-shift codebase
    11:51  + Created ~/projects/nursery-shift/src/nr_supply.py
    11:51  $ Syntax check nr_supply.py
    11:53  + Created ~/projects/nursery-shift/src/shift_milp.py
    11:54  ~ Modified ~/projects/nursery-shift/src/scheduler.py
    11:55  ~ Modified ~/projects/nursery-shift/src/schedule_optimizer.py
    12:10  G Git commit
    ...

  ▸ Risk Flags
    None detected

  ▸ Files Touched
    NEW  ~/projects/nursery-shift/src/nr_supply.py
    NEW  ~/projects/nursery-shift/src/shift_milp.py
    MOD  ~/projects/nursery-shift/src/scheduler.py
    MOD  ~/projects/nursery-shift/src/schedule_optimizer.py
```

## Usage

```bash
# Most recent session (default)
npx cc-audit-log

# All sessions from today
npx cc-audit-log --today

# Sessions from a specific date
npx cc-audit-log --date 2026-02-27

# Last N sessions
npx cc-audit-log --last 5

# All sessions (can be slow for heavy users)
npx cc-audit-log --all
```

## Risk detection

The tool flags potentially risky commands:

| Pattern | Flag |
|---------|------|
| `rm -rf` | Recursive delete |
| `git push --force` | Force push |
| `git reset --hard` | Hard reset |
| `npm publish` | npm publish |
| `sudo` | Sudo command |
| `curl -X POST` | HTTP POST request |
| `DROP TABLE/DATABASE` | Database drop |

## How it works

1. Scans `~/.claude/projects/` for session transcript files (.jsonl)
2. Parses each line for `tool_use` events (assistant messages)
3. Classifies actions: file writes, edits, bash commands, git operations
4. Generates a human-readable timeline with risk flags
5. Includes subagent sessions

**Zero dependencies. No data sent anywhere. Runs entirely local.**

## The cc-toolkit trilogy

| Tool | What it checks |
|------|---------------|
| [cc-health-check](https://github.com/yurukusa/cc-health-check) | Is your AI **setup** safe? |
| [cc-session-stats](https://github.com/yurukusa/cc-session-stats) | How much are you **using** AI? |
| **cc-audit-log** | What did your AI **do**? |

## License

MIT
