# find-session

Search and resume coding sessions across AI agents — [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Codex](https://github.com/openai/codex), and [OpenCode](https://github.com/opencode-ai/opencode) — from a single command.

Sessions from all agents are merged into one table sorted by time, with keyword search (AND logic), project filtering, and an interactive selection UI for resuming, inspecting, or exporting sessions.

## Install

```
pip install find-session
```

or with [uv](https://docs.astral.sh/uv/):

```
uv tool install find-session
```

## Example

Searching across all agents and projects:

```
$ find-session "auth" -g -n 5
```

```
                            Sessions matching: auth
╭───┬────────┬────────────┬──────────┬────────┬─────────────┬───────┬──────────────────────────────────╮
│ # │ Agent  │ Session ID │ Project  │ Branch │ Date        │ Lines │ Last User Message                │
├───┼────────┼────────────┼──────────┼────────┼─────────────┼───────┼──────────────────────────────────┤
│ 1 │ Claude │ a28d83f8…  │ myapp    │ main   │ 02/21 16:01 │   174 │ Can you add OAuth2 support to    │
│   │        │            │          │        │             │       │ the login flow?                  │
│ 2 │ OC     │ ses_37f6…  │ myapp    │ N/A    │ 02/21 15:48 │   216 │ refactor the auth middleware to  │
│   │        │            │          │        │             │       │ use JWT tokens instead           │
│ 3 │ Claude │ 0585e460…  │ backend  │ feat   │ 02/20 11:30 │  3179 │ debug the session expiry bug     │
│ 4 │ Codex  │ 0199bfc9…  │ myapp    │ main   │ 02/19 09:15 │    42 │ add rate limiting to auth        │
│   │        │            │          │        │             │       │ endpoints                        │
│ 5 │ OC     │ ses_3c18…  │ backend  │ N/A    │ 02/18 20:25 │   235 │ write tests for the new auth     │
│   │        │            │          │        │             │       │ module                           │
╰───┴────────┴────────────┴──────────┴────────┴─────────────┴───────┴──────────────────────────────────╯

Select a session:
  • Enter number (1-5) to select
  • Press Enter to cancel
```

After selecting a session, you get an action menu:

```
=== Session: a28d83f8... ===
Agent: Claude
Project: myapp

What would you like to do?
1. Resume session (default)
2. Show session file path
3. Copy session file to file (*.jsonl) or directory
```

## Usage

```
find-session [keywords] [OPTIONS]
```

| Flag | Description |
|------|-------------|
| `keywords` | Comma-separated search terms (AND logic). Omit to show all sessions. |
| `-g, --global` | Search across all projects, not just the current one |
| `-n, --num-matches N` | Number of results to display (default: 10) |
| `--agents claude codex opencode` | Limit search to specific agents |
| `--shell` | Output shell commands for `eval` (enables persistent `cd`) |
| `--claude-home PATH` | Custom Claude home directory (default: `~/.claude`) |
| `--codex-home PATH` | Custom Codex home directory (default: `~/.codex`) |
| `--opencode-home PATH` | Custom OpenCode data directory (default: `~/.local/share/opencode`) |

### Examples

```bash
# Search current project across all agents
find-session "error,TypeError"

# Browse all sessions globally
find-session -g

# Search only Claude and OpenCode sessions
find-session "refactor" --agents claude opencode

# Use with shell wrapper for persistent directory changes
fs() { eval $(find-session --shell "$@"); }
fs "keyword" -g
```

### Standalone commands

Each agent also has a dedicated command:

```bash
find-claude-session "keywords"       # Claude Code only
find-codex-session "keywords"        # Codex only
find-opencode-session "keywords"     # OpenCode only
```

## Supported agents

| Agent | Session storage | Resume method |
|-------|----------------|---------------|
| **Claude Code** | JSONL files in `~/.claude/projects/` | `claude -r <session_id>` |
| **Codex** | JSONL rollouts in `~/.codex/sessions/` | `codex resume <session_id>` |
| **OpenCode** | SQLite database at `~/.local/share/opencode/opencode.db` | `opencode --session <session_id>` |

## Configuration

Optionally create `~/.config/find-session/config.json` to customise agent display names, set custom home directories, or disable agents:

```json
{
  "agents": [
    { "name": "claude", "display_name": "Claude", "enabled": true },
    { "name": "codex", "display_name": "Codex", "enabled": false },
    { "name": "opencode", "display_name": "OC", "enabled": true }
  ]
}
```

## Origin

This project grew out of session-finding utilities in [claude-code-tools](https://github.com/pchalasani/claude-code-tools), extracted into a standalone package to make it easy to install and maintain independently.

## License

MIT
