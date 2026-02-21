# find-session

Search and resume sessions across coding agents: Claude Code, Codex, and OpenCode.

## Install

```
uv tool install "find-session @ git+https://github.com/charlesnchr/find-session"
```

## Usage

```
find-session "keywords"           # Search all agents in current project
find-session -g                   # All sessions across all projects
find-session "bug" --agents claude opencode  # Filter by agent
find-claude-session "keywords"    # Claude only
find-codex-session "keywords"     # Codex only
find-opencode-session "keywords"  # OpenCode only
```
