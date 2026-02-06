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
/plugin marketplace add xiaolai/init-workspace
/plugin install init-workspace@xiaolai-init-workspace
```

Then use `/init-workspace:init-workspace` to run.

### Option 2: Personal Skill (Simpler)

```bash
mkdir -p ~/.claude/skills/init-workspace
curl -o ~/.claude/skills/init-workspace/SKILL.md \
  https://raw.githubusercontent.com/xiaolai/init-workspace/main/skills/init-workspace/SKILL.md
```

Or clone the entire repo:

```bash
git clone https://github.com/xiaolai/init-workspace /tmp/iw && \
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
│   ├── skills/              # Claude Code skills
│   └── rules/               # Modular rules
├── .codex/
│   ├── skills/              # Codex CLI skills
│   └── prompts/             # Codex CLI custom slash commands
├── .gemini/
│   ├── skills/              # Gemini CLI skills
│   └── commands/            # Gemini CLI custom slash commands (TOML)
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
| `.claude/skills/` | Claude Code skills (slash commands) |
| `.claude/rules/` | Modular rules auto-loaded into context |
| `.codex/skills/` | Codex CLI skills |
| `.codex/prompts/` | Codex CLI custom slash commands |
| `.gemini/skills/` | Gemini CLI skills |
| `.gemini/commands/` | Gemini CLI custom slash commands (TOML) |
| `.mcp.json` | MCP server configuration |
| `.gitignore` | Comprehensive ignore rules |

## Customization

After initialization:

1. Edit `AGENTS.md` to add project-specific instructions
2. Add skills to `.claude/skills/`, `.codex/skills/`, or `.gemini/skills/`
3. Add subagents to `.claude/agents/`, rules to `.claude/rules/`
4. Configure MCP servers in `.mcp.json`

## License

MIT

## Sources

- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)
- [Claude Code Plugins Documentation](https://code.claude.com/docs/en/plugins)
- [Codex CLI Skills Documentation](https://developers.openai.com/codex/skills/)
- [Gemini CLI Skills Documentation](https://geminicli.com/docs/cli/skills/)
- [AGENTS.md Open Format](https://agents.md/)
