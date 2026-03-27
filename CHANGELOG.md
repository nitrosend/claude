# Changelog

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
