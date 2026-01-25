# init-workspace

A Claude Code plugin that initializes a multi-agent workspace with shared configuration for Claude Code, Codex CLI, and Gemini CLI.

## Installation

### Option 1: Plugin Marketplace (Recommended)

In Claude Code, run:

```
/plugin marketplace add xiaolai/claude-init-workspace
/plugin install init-workspace@xiaolai-claude-init-workspace
```

Then use `/init-workspace:init-workspace` to run.

### Option 2: Personal Skill (Simpler)

```bash
mkdir -p ~/.claude/skills/init-workspace
curl -o ~/.claude/skills/init-workspace/SKILL.md \
  https://raw.githubusercontent.com/xiaolai/claude-init-workspace/main/skills/init-workspace/SKILL.md
```

Or clone the entire repo:

```bash
git clone https://github.com/xiaolai/claude-init-workspace /tmp/iw && \
  mkdir -p ~/.claude/skills/init-workspace && \
  cp /tmp/iw/skills/init-workspace/SKILL.md ~/.claude/skills/init-workspace/ && \
  rm -rf /tmp/iw
```

Then use `/init-workspace` to run.

## Usage

In any project directory, run:

```
/init-workspace                         # if installed as personal skill
/init-workspace:init-workspace          # if installed as plugin
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

- `/doc <topic>` - Create a timestamped markdown document in `dev-docs/` and open in VMark

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

## Sources

- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)
- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
- [Discover and Install Plugins](https://code.claude.com/docs/en/discover-plugins)
