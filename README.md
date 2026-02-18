# Claude Configuration

Personal Claude configuration repository for syncing settings, agents, commands, and customizations across machines.

## Installation

### 1. Clone this repository

```bash
git clone https://github.com/davenicoll/claude.git
```

### 2. Create symlink to ~/.claude

```bash
# Remove existing ~/.claude directory if it exists (backup first if needed)
rm -rf ~/.claude

# Create symlink from this repo to ~/.claude
ln -s /Users/[username]/Source/davenicoll/claude ~/.claude
```

### 3. Verify the symlink

```bash
ls -la ~/.claude
# Should show: .claude -> /Users/[username]/Source/davenicoll/claude
```

## Directory Structure

- `agents/` - Custom agent definitions (subagents spawned via the Task tool)
- `commands/` - Custom slash commands (invoked via `/command-name`)
- `ide/` - IDE integration settings
- `plugins/` - Plugin configurations
- `settings.json` - Global Claude settings
- `settings.local.json` - Local machine-specific settings (gitignored)
- `CLAUDE.md` - Global user instructions loaded into every session

## Commands

| Command | Description |
|---|---|
| `/clean-settings` | Audits and cleans `settings.local.json` — validates structure, ordering, and scans for leaked sensitive data |
| `/agents-md` | Creates, analyzes, or fixes AGENTS.md / CLAUDE.md files based on ETH Zurich research on effective context files |

## Agents

| Agent | Description |
|---|---|
| `doctor` | Evidence-based medical research, treatment evaluation, and health claim assessment |
| `storyteller` | Crafts compelling narratives for presentations, pitches, and complex explanations |
| `editor-anti-llm` | Reviews and edits prose (docs, READMEs, blog posts) to ensure quality and avoid AI writing patterns |
| `writer-chapters` | Produces book chapters and long-form structured writing with narrative consistency |
| `editor-in-chief` | Comprehensive quality analysis for narrative content, research findings, and anti-LLM pattern detection |

## Settings Priority

Settings are loaded in the following order, with higher priority files overriding lower ones:

| Priority | File | Purpose |
|---|---|---|
| 1 (highest) | Managed (`/Library/Application Support/ClaudeCode/managed-settings.json`) | Org-wide policies, cannot be overridden |
| 2 | CLI arguments | Session-specific overrides |
| 3 | Local (`.claude/settings.local.json`) | Personal project overrides (gitignored) |
| 4 | Project (`.claude/settings.json`) | Team-shared settings (committed to git) |
| 5 (lowest) | User (`~/.claude/settings.json`) | Personal global defaults |
