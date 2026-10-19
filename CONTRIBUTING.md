# Nitrosend Claude Plugin

## Conventions

- Use Conventional Commits with the scopes `plugin`, `skill`, `agent`, `hook`,
  or `mcp`.
- Do not add co-authored-by or AI trailers.
- Skills and agents use valid YAML frontmatter.
- Treat the tool catalog and schemas returned by the production MCP session as
  authoritative. Do not hardcode a tool count or offer unavailable tools.
- Treat `nitro_search_docs` output as the authority for product details not
  established by the live tool contract.
- Document only the current client and transport behavior. Git history is the
  release-history archive.

## Structure

- `.claude-plugin/plugin.json` — plugin manifest
- `.mcp.json` — production remote MCP server definition
- `SETUP.md` — OAuth setup, account selection, and troubleshooting
- `hooks/hooks.json` — plugin hook configuration
- `agents/` — plugin agent definitions
- `skills/` — guided Nitrosend workflows
