# /init-workspace

A Claude Code skill that initializes a multi-agent workspace with shared configuration for Claude Code, Codex CLI, and Gemini CLI.

## Installation

```bash
# Clone to your global Claude Code skills directory
git clone https://github.com/xiaolai/claude-init-workspace ~/.claude/skills/init-workspace
```

Or manually copy `SKILL.md` to `~/.claude/skills/init-workspace/SKILL.md`.

## Usage

In any project directory, run:

```
/init-workspace
```

Claude will ask for a project description, then create:

```
project/
├── .claude/
│   ├── settings.json        # Team-shared settings
│   ├── agents/              # Custom subagents
│   ├── skills/doc/          # /doc command included
│   └── rules/               # Modular rules
├── dev-docs/                # Generated documentation
├── .mcp.json                # MCP server configuration
├── .gitignore               # Comprehensive gitignore
├── AGENTS.md                # Shared instructions (all tools read this)
├── CLAUDE.md                # Claude Code (imports AGENTS.md)
└── GEMINI.md                # Gemini CLI (imports AGENTS.md)
```

## Features

### Unified Instructions

All three AI tools share the same instructions via `AGENTS.md`:
- **Claude Code** reads `CLAUDE.md` which imports `@AGENTS.md`
- **Codex CLI** reads `AGENTS.md` natively
- **Gemini CLI** reads `GEMINI.md` which imports `@AGENTS.md`

New instructions are always written to `AGENTS.md` only, ensuring consistency.

### Included Skills

- `/doc <topic>` - Create a timestamped markdown document in `dev-docs/` and open it in VMark

### Comprehensive .gitignore

Covers AI tools, secrets, OS files, editors, dependencies, build outputs, logs, and test coverage.

## Customization

After initialization:

1. Edit `AGENTS.md` to add project-specific instructions
2. Add custom subagents to `.claude/agents/`
3. Add custom skills to `.claude/skills/`
4. Add modular rules to `.claude/rules/`
5. Configure MCP servers in `.mcp.json`

## License

MIT
