---
name: init-workspace
description: Initialize a workspace with shared AI agent configuration files for Claude Code, Codex CLI, and Gemini CLI, plus full Claude Code directory structure
disable-model-invocation: true
---

# Initialize Multi-Agent Workspace

Create shared configuration files for Claude Code, Codex CLI, and Gemini CLI, plus the complete Claude Code project structure.

## Instructions

1. Ask the user two questions:
   - "Single line description of your project:"
   - "Keep AI config files private or public?"
     - **Private**: AI files (AGENTS.md, CLAUDE.md, GEMINI.md, .claude/, .mcp.json, dev-docs/) are gitignored. Use when you don't want to share AI instructions with collaborators.
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
│   │   └── doc/
│   │       └── SKILL.md     # /doc command
│   └── rules/               # Modular rules
│       └── .gitkeep
├── dev-docs/                # Generated documentation
│   └── .gitkeep
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
- `.claude/skills/` - Slash commands (e.g., `/doc`, `/test`)
- `.claude/rules/` - Modular rules auto-loaded into context
- `.mcp.json` - MCP server configuration
- `dev-docs/` - Generated documentation

## Markdown Documentation

When producing substantial markdown content (documentation, explanations, guides, analyses):

1. Save to `dev-docs/` with filename: `YYYYMMDD-HHMMSS-<descriptive-slug>.md`
2. Open with: `open -a VMark <filepath>`

This applies to technical docs, design decisions, code explanations, architecture overviews.
Short answers or code snippets do not need to be saved.

Use `/doc <topic>` to manually create and open a document.
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

#### .claude/skills/doc/SKILL.md
```markdown
# Create Documentation

Create a markdown document in dev-docs/ and open it in VMark.

## Usage

```
/doc <topic>
```

## Instructions

1. Generate a descriptive slug from the topic (lowercase, hyphens, no special chars)
2. Create filename: `YYYYMMDD-HHMMSS-<slug>.md`
3. Write the markdown file to `dev-docs/`
4. Open the file with: `open -a VMark <filepath>`
5. Confirm the file was created and opened

If no topic is provided, ask the user what they want to document.
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

# Codex CLI - local configuration directory
.codex/

# Gemini CLI - local configuration directory
.gemini/

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
.mcp.json
dev-docs/
```

3. Create .gitkeep files in empty directories to ensure they're tracked by git.

4. Tell the user:
   - "Workspace initialized for Claude Code, Codex CLI, and Gemini CLI."
   - If PUBLIC: "AI config files will be shared with your team via git."
   - If PRIVATE: "AI config files are gitignored and won't be shared."
   - "Edit AGENTS.md to add shared instructions for all AI tools."
   - "Add subagents to .claude/agents/, skills to .claude/skills/, rules to .claude/rules/"
   - "Configure MCP servers in .mcp.json"
   - "Use `/doc <topic>` to create documentation in dev-docs/"
