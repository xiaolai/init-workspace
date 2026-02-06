---
name: init-workspace
description: Initialize a workspace with shared AI agent configuration files for Claude Code, Codex CLI, and Gemini CLI, plus full directory structure for all three tools
disable-model-invocation: true
---

# Initialize Multi-Agent Workspace

Create shared configuration files for Claude Code, Codex CLI, and Gemini CLI, plus the project structure for all three tools.

## Instructions

1. Ask the user two questions:
   - "Single line description of your project:"
   - "Keep AI config files private or public?"
     - **Private**: AI files (AGENTS.md, CLAUDE.md, GEMINI.md, .claude/, .codex/skills/, .codex/prompts/, .gemini/skills/, .gemini/commands/, .mcp.json) are gitignored. Use when you don't want to share AI instructions with collaborators.
     - **Public** (recommended): AI files are committed to the repo. Use when you want the team to share the same AI context.

2. Create the following directory structure and files:

### Directory Structure

```
project/
├── .claude/
│   ├── settings.json        # Team-shared settings
│   ├── agents/              # Custom subagents
│   │   └── .gitkeep
│   ├── skills/              # Custom skills
│   │   └── .gitkeep
│   └── rules/               # Modular rules
│       └── .gitkeep
├── .codex/
│   ├── skills/              # Codex CLI skills
│   │   └── .gitkeep
│   └── prompts/             # Codex CLI custom slash commands
│       └── .gitkeep
├── .gemini/
│   ├── skills/              # Gemini CLI skills
│   │   └── .gitkeep
│   └── commands/            # Gemini CLI custom slash commands (TOML)
│       └── .gitkeep
├── .mcp.json                # MCP server configuration
├── .gitignore               # Comprehensive gitignore
├── AGENTS.md                # Shared instructions (Codex CLI native)
├── CLAUDE.md                # Claude Code (imports AGENTS.md)
└── GEMINI.md                # Gemini CLI (imports AGENTS.md)
```

### File Contents

#### AGENTS.md
```markdown
# Project Instructions

> $USER_RESPONSE

## Guidelines

<!-- Add your project-specific instructions here -->

## Shared Memory

**Always write new instructions, rules, and memory to `AGENTS.md` only.**

Never modify `CLAUDE.md` or `GEMINI.md` directly - they only import `AGENTS.md`.
This ensures Claude Code, Codex CLI, and Gemini CLI share the same context consistently.

## Project Structure

- `.claude/agents/` - Custom subagents for specialized tasks
- `.claude/skills/` - Claude Code skills (slash commands)
- `.claude/rules/` - Modular rules auto-loaded into context
- `.codex/skills/` - Codex CLI skills
- `.codex/prompts/` - Codex CLI custom slash commands
- `.gemini/skills/` - Gemini CLI skills
- `.gemini/commands/` - Gemini CLI custom slash commands (TOML)
- `.mcp.json` - MCP server configuration
```

#### CLAUDE.md
```markdown
@AGENTS.md
```

#### GEMINI.md
```markdown
@AGENTS.md
```

#### .mcp.json
```json
{
  "mcpServers": {}
}
```

#### .claude/settings.json
```json
{
  "permissions": {
    "allow": [],
    "deny": []
  }
}
```

#### .gitignore (append if exists, create if not)

**If user chose PUBLIC (recommended):**

```gitignore
# =============================================================================
# AI CODING ASSISTANTS
# =============================================================================

# Claude Code - local settings contain personal preferences and may include tokens
.claude/settings.local.json
.claude/CLAUDE.local.md
.claude/*.local.*
CLAUDE.local.md

# Codex CLI - local files (sessions, logs, config)
.codex/*
!.codex/skills/
!.codex/prompts/

# Gemini CLI - local files (settings, tmp)
.gemini/*
!.gemini/skills/
!.gemini/commands/

# =============================================================================
# SECRETS & ENVIRONMENT
# =============================================================================

# Environment files often contain API keys, database credentials, etc.
.env
.env.*
!.env.example              # Keep example files for documentation

# Private keys and certificates
*.pem
*.key

# Credential files
credentials.json
secrets.json

# =============================================================================
# OPERATING SYSTEM
# =============================================================================

# macOS - Finder metadata
.DS_Store

# Windows - thumbnail cache
Thumbs.db

# =============================================================================
# EDITORS & IDEs
# =============================================================================

# JetBrains (IntelliJ, WebStorm, PyCharm, etc.)
.idea/

# Visual Studio Code
.vscode/

# Vim/Neovim swap and backup files
*.swp
*.swo
*~

# =============================================================================
# DEPENDENCIES
# =============================================================================

# Node.js
node_modules/

# PHP (Composer)
vendor/

# Python
__pycache__/
*.pyc
.venv/
venv/

# =============================================================================
# BUILD OUTPUTS
# =============================================================================

# Common build directories
dist/
build/

# Python package build
*.egg-info/

# Rust/Cargo
target/

# =============================================================================
# LOGS & TEMPORARY FILES
# =============================================================================

*.log
logs/

# =============================================================================
# TEST COVERAGE
# =============================================================================

# JavaScript/TypeScript
coverage/
.nyc_output/

# Python
htmlcov/
```

**If user chose PRIVATE:**

Use the same gitignore as above, but ADD these lines to the AI CODING ASSISTANTS section:

```gitignore
# AI config files (private mode - not shared with team)
AGENTS.md
CLAUDE.md
GEMINI.md
.claude/
.codex/skills/
.codex/prompts/
.gemini/skills/
.gemini/commands/
.mcp.json
```

3. Create .gitkeep files in empty directories to ensure they're tracked by git.

4. Tell the user:
   - "Workspace initialized for Claude Code, Codex CLI, and Gemini CLI."
   - If PUBLIC: "AI config files will be shared with your team via git."
   - If PRIVATE: "AI config files are gitignored and won't be shared."
   - "Edit AGENTS.md to add shared instructions for all AI tools."
   - "Add skills to .claude/skills/, .codex/skills/, or .gemini/skills/"
   - "Add subagents to .claude/agents/, rules to .claude/rules/"
   - "Configure MCP servers in .mcp.json"
