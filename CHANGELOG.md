# Changelog

## 2.1.0 (2026-08-03)

### Improvements

- Documented the full 28-tool registered MCP surface (was 23): added `nitro_inbox`, `nitro_inbox_action`, `nitro_ingest`, `nitro_manage_outreach`, `nitro_search_docs`, and `nitro_select_account`
- Marked the capability-gated early-access tools (`nitro_inbox`, `nitro_inbox_action`, `nitro_manage_outreach`) — they appear only on accounts with the feature enabled
- Documented the composition contract now enforced by the compose tools (`composition_mode: "intent"` → optional `"validate"` → `"draft"`, or metered `"generate"`) in the compose-email, send-campaign, and build-flow skills and the email-marketer agent
- Added `nitro_select_account` switching guidance (setup skill + SETUP.md) and `nitro_search_docs` as the authoritative product-docs lookup
- Added `nitro_ingest` image-hosting guidance (base64 for small images, direct-upload reservation for larger files, hosted URL into the `image` section `src`)
- Extended the email-marketer agent tool table to all 28 tools

### Fixes

- Corrected the stale CLAUDE.md note that still described the removed SessionStart auto-status hook
- Corrected the "Knows all 23 Nitrosend MCP tools" claim to 28

## 2.0.0 (2026-04-20)

### Breaking Changes

- Hard-cut the Claude plugin over from the deprecated local bridge to the canonical remote Nitrosend MCP server at `https://api.nitrosend.com/mcp`
- Switched plugin setup from API-key bootstrap to an OAuth-first connection flow for Claude clients

### Fixes

- Corrected the live Nitrosend tool surface from 20 to 21 tools
- Replaced stale tool references with the live names: `nitro_manage_template`, `nitro_review_delivery`, and `nitro_send_test_message`
- Corrected the sandbox domain reference to `nitr-o.com`
- Removed the broken SessionStart prompt hook that current Claude Code rejects for `SessionStart`

### Improvements

- Added `SETUP.md` with connect, reconnect, disconnect, and `nitro_get_status` verification guidance
- Refreshed plugin README and internal notes to describe the bundled remote MCP transport accurately

### Validation Notes

- Current-build validation passed on Claude Code `2.1.114` via `claude plugin validate ./claude`, and `--plugin-dir ./claude` loaded the Nitrosend slash commands while surfacing Nitrosend as an auth-required MCP server
- The system `claude` binary on `PATH` remained `1.0.119`, which exposed `claude mcp` but not the newer `/plugin` flow until migrated to the newer install
- Remote OAuth discovery remained live during the cutover at `/.well-known/oauth-authorization-server` and `/.well-known/oauth-protected-resource/mcp`

## 1.1.0 (2026-03-27)

### Fixes

- Renamed `nitro_preview_and_test` to `nitro_review_and_test` across all skills and agent to match actual MCP tool name
- Fixed `operation: "preview"` to `operation: "review"` in compose-email and send-campaign skills
- Updated tool count from "16+" to 20 (accurate count of all MCP tools)
- Added missing `nitro_request_support` to agent tool table

### Improvements

- Removed `settings.json` default agent override to avoid hijacking non-email sessions
- Added `assets/` directory and `.gitignore` per monorepo conventions
- Updated README quick start and requirements

## 1.0.0 (2026-03-25)

Initial release.

### Features

- MCP bridge to `@nitrosend/mcp` (16+ email marketing tools)
- Email Marketer agent with email marketing bible knowledge preloaded
- 8 skills: setup, compose-email, send-campaign, send-transactional, build-flow, import-contacts, analytics, email-marketing-bible
- SessionStart hook for automatic onboarding status check
- Proactive analytics setup (daily/weekly/monthly scheduled reports)
