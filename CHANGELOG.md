# Changelog

## 2.2.0 (2026-10-27)

- Renamed the generic `setup` and `analytics` skills to `nitrosend-setup` and
  `nitrosend-analytics` so they stay unambiguous in shared skill directories.
  The commands are now `/nitrosend:nitrosend-setup` and
  `/nitrosend:nitrosend-analytics`.
- Removed the vendored `email-marketing-bible` skill. Install it from
  `CosmoBlk/email-marketing-bible` instead.
- Added a Google Antigravity package: `.antigravity-plugin/plugin.json` and a
  root `mcp_config.json` that uses `serverUrl`. Install with
  `agy plugin install https://github.com/nitrosend/claude`.
- The `email-marketer` agent now inherits the session model, which both Claude
  Code and Antigravity accept.
- Removed the empty `hooks/hooks.json`. The plugin registers no hooks.
- Added `privacyPolicyUrl` to the plugin manifest and documented
  `npx skills add nitrosend/claude`, privacy and support in the README.

## 2.1.5 (2026-10-19)

- Moved repository-only contributor conventions out of plugin context discovery so strict Claude plugin validation passes.
- Refreshed the bundled desktop extension dependencies to resolve audited vulnerabilities and prepared MCPB 1.0.1.

## 2.1.4 (2026-08-20)

- Replaced retired shared-sender setup with the live on-demand brand-subdomain
  preparation, readiness polling, and explicit sender-selection workflow.
- Documented retry-safe first-send provisioning for transactional messages and
  made the API's live readiness findings authoritative for campaign delivery.

## 2.1.3 (2026-08-12)

- Synced skill and agent recipes with the live MCP contract: every
  `nitro_control_delivery` call now documents the required
  `expected_brand_sid` brand assertion (plus explicit `target_type` +
  `target_id`), send-campaign covers audience exclusions
  (`exclude_contact_list_ids` / `exclude_segment_ids`), and build-flow covers
  per-email route selection (`email_baseline_selections`, incl.
  `image_binding_ids` for image-led routes).
- Made this repository an installable plugin marketplace
  (`.claude-plugin/marketplace.json`); README install now uses
  `/plugin marketplace add nitrosend/claude`.
- README: plugin agents are managed via `/agents`; noted inline email
  previews for compose/template results in MCP-Apps-capable hosts.

- Added `mcpb/`: a Claude Desktop extension (MCPB) bundling an mcp-remote-based
  launcher for the production remote MCP server — browser OAuth by default,
  optional sensitive API-key setting (env-indirected, never in argv), dedicated
  OAuth token cache under `~/.nitrosend/mcpb-auth`, and a manifest declaring
  the public tool baseline with `privacy_policies`.

## 2.1.2 (2026-08-04)

- Made the intent scaffold and `nitro://schema` authoritative for the complete
  email-section registry, preventing the compose skill's illustrative patterns
  from becoming a stale exhaustive schema.

## 2.1.1 (2026-08-04)

- Made the live production MCP catalog and schemas authoritative throughout the
  plugin.
- Updated the email composition workflow to preserve the server-selected
  complete baseline, creative route, evidence bindings, image choice, and
  stable persistence key.
- Corrected domain parameter nesting, current BYO provider coverage, canonical
  merge tags and footer ownership, image descriptions, and transactional
  idempotency examples.
- Removed unavailable marketplace instructions, retired transport guidance,
  fixed tool-count claims, and volatile benchmark, vendor, legal, and pricing
  assertions.
- Clarified that capability-gated tools and host scheduling are offered only
  when the connected session exposes them.

Earlier release history remains available in Git.
