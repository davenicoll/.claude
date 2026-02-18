# Claude Configuration Repository

This repo is symlinked to `~/.claude` and contains user-level Claude Code configuration. Changes here affect all projects on this machine.

## Key files

- `settings.json` - Global user settings (priority 5, committed to git)
- `settings.local.json` - Local machine-specific overrides (priority 3, gitignored)
- `agents/` - Custom agent definitions
- `commands/` - Custom slash commands
