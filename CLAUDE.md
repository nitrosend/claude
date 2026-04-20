# Nitrosend Claude Plugin

## Conventions

- Conventional Commits: `type(scope): description`
- Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`
- Scopes: `plugin`, `skill`, `agent`, `hook`, `mcp`
- No co-authored-by or AI trailers in commits
- Skills use YAML frontmatter with `name` and `description` fields
- Agent definitions use YAML frontmatter per Claude Code subagent spec

## Structure

- `.claude-plugin/plugin.json` — manifest (name, version, metadata)
- `.mcp.json` — remote Nitrosend MCP server definition for `https://api.nitrosend.com/mcp`
- `SETUP.md` — guided OAuth setup and reconnect instructions for the bundled MCP server
- `hooks/hooks.json` — SessionStart auto-status check
- `agents/` — email-marketer agent definition
- `skills/` — 8 skills (setup, compose-email, send-campaign, send-transactional, build-flow, import-contacts, analytics, email-marketing-bible)
