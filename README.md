# init-workspace

Initialize any project for AI-assisted development with a single command.

## The Problem

You're using multiple AI coding tools — Claude Code, OpenAI Codex CLI, Google Gemini CLI. Each has its own config file. You end up with:

- Instructions scattered across `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`
- Each tool learning things the others don't know
- Inconsistent behavior when switching between tools
- Missing `.gitignore` entries that leak secrets or bloat repos
- No structure for custom skills, agents, or rules

## The Solution

Run `/init-workspace` once. Get a project structure where:

- **One file to rule them all**: Write instructions in `AGENTS.md` only. Claude Code, Codex CLI, and Gemini CLI all read from it automatically.
- **Shared memory**: When any tool learns something new, it updates `AGENTS.md`. All tools stay in sync.
- **Ready for customization**: Directories for skills, agents, and rules are already in place.
- **Safe defaults**: Comprehensive `.gitignore` protects secrets and keeps repos clean.

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

Claude will ask two questions:

1. **Project description** — A single line describing your project
2. **Public or Private** — Whether to share AI config files via git:
   - **Public** (recommended): Team shares the same AI context
   - **Private**: AI files are gitignored, each dev has their own

Then it creates:

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

## What Gets Created

| File/Directory | Purpose |
|----------------|---------|
| `AGENTS.md` | Your single source of truth for all AI tools |
| `CLAUDE.md` | Imports `@AGENTS.md` (Claude Code reads this) |
| `GEMINI.md` | Imports `@AGENTS.md` (Gemini CLI reads this) |
| `.claude/settings.json` | Team-shared permissions |
| `.claude/agents/` | Custom subagents |
| `.claude/skills/` | Custom slash commands |
| `.claude/rules/` | Modular rules auto-loaded into context |
| `.mcp.json` | MCP server configuration |
| `dev-docs/` | AI-generated documentation |
| `.gitignore` | Comprehensive ignore rules |

### Included Skills

- `/doc <topic>` — Create a timestamped markdown document in `dev-docs/`

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
