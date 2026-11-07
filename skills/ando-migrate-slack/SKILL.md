---
name: ando-migrate-slack
description: Move Slack history and ongoing conversations into Ando using available MCP tools or documented APIs, verified mappings, and a bounded pilot. Use for Slack migration, channel discovery, history backfill, or sync setup.
---

# Migrate from Slack

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-migrate-slack"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Use MCP first, then a documented API with authorized credentials. Keep discovery,
channel selection, execution, and readback in chat wherever those operations are
available. Use a browser for required OAuth consent or review, or an explicitly
chosen UI fallback.

Read the [shared migration workflow](../../references/migration-workflow.md).
Use the current [Slack import and sync guide](https://docs.ando.so/docs/importing-from-slack)
for coverage and fidelity. Source access, destination access, history import,
and ongoing sync are separate capabilities; verify each needed by the request.

## Resolve access and executable capabilities

Carry forward the source workspace, target Ando URL, history/sync choice, and
already authorized scope. Ask only for missing choices after reading available
options. Showing pilot channels authorizes discovery, not starting a migration
or silently choosing a channel.

1. Verify the Ando principal and workspace through the exposed identity read.
   Resolve the target URL to a verified workspace identity; a slug is not an ID.
   If it differs, follow [connection and principal](../../references/connection-and-principal.md)
   using the exact plugin connection. Initiate its supported reconnect flow when
   available, then refresh tools and verify the calling chat. Website sign-in,
   a separate browser profile, and helper OAuth success do not switch the plugin.
2. Discover current MCP schemas for Slack connection status, channel listing,
   destination mappings, migration execution, and progress. These are capabilities
   to look for, not assumed tool names. Compare server-advertised capabilities
   with the host's callable tools. Discover deferred tools when supported;
   distinguish missing grants, wrong workspace, host refresh, and unimplemented
   operations before proposing a remedy.
3. Inspect the provider catalog before using generic connected-app tools. Native
   Slack sync may be separate from that catalog. Do not pass `slack` to a provider
   operation unless its schema/catalog advertises it. A rejected provider name
   does not establish that native Slack sync is unavailable.
4. If MCP lacks a needed operation, inspect the current
   [OpenAPI specification](https://api.ando.so/openapi.json) and
   [history import contract](https://docs.ando.so/developers/api-reference#import-history).
   Use only documented endpoints, schemas, authentication, and scopes supported
   by the credential. MCP OAuth access is not proof of public API access. Never
   extract browser cookies, reuse internal app-session routes, or call private
   Convex functions as an API fallback.

A Slack connector may provide authorized source reads even when Ando cannot
list source channels. Verify its Slack team identity against the requested
workspace before reading, and label its results as source-visible channels;
they are not proof that Ando's Slack integration can import those channels.
Source discovery can proceed independently while target authorization is pending.

## Native programmatic workflow when advertised

If the current catalog exposes `manage_slack_migration`, use its advertised
schema. The equivalent documented endpoint is `POST /v1/slack/migration`.
This capability requires a human API key with `slack_migration:read`, plus
`slack_migration:write` for consent initiation, plans, apply and retry. It is
workspace-gated; ordinary MCP OAuth or an agent credential does not imply these
grants. Server rollout must precede relying on this guidance.

1. Call `action: "connection"` and verify the workspace and Slack team. If
   disconnected, use `begin_connection`, return the actual authorization URL,
   and resume `get_connection_attempt` with its reference. Check the actual
   resulting workspace; a team claim can route consent elsewhere.
2. Use `list_channels` and follow `next_cursor`. A changed-directory error means
   restart pagination; `truncated: true` means the provider directory is partial
   even after the last page. Installer membership/activity is explicitly labeled.
3. Submit `plan` with `expected_slack_team_id` and up to 50 explicit selections:
   source Slack ID, an existing destination ID or a new name, `live_sync`, and
   `backfill_history`. Show the returned pairs and blockers. Planning does not
   start the onboarding preview or write channels, links, or history.
4. Apply only the authorized, unblocked plan using its ID/version and a stable
   idempotency key. Plans expire after 24 hours. Destination/name or grant drift
   requires a fresh plan. Never silently substitute another destination.
5. Poll `get_operation` by operation ID. Report setup, live sync, and history
   independently. Use `retry` only for explicitly retryable failed item IDs,
   retaining successful results. Do not retry by creating another migration.

The initial programmatic pilot maps internal public channels. Private and Slack
Connect channels can appear in discovery but carry audience-review blockers.
Mapping-only creates paused links; history requires live sync. The pilot requires
the Slack installer to already belong to a channel before enabling live sync;
`slack_installer_not_in_channel` is a blocker, not a successful sync. Existing mappings
cannot be retargeted through this capability. Keep unsupported selections visible
and explain their blocker instead of falling back to broader writes.

## Show channels and prepare the pilot

Use the authorized channel-list operation and follow its pagination. Show channel
name, stable ID, public/private/Slack Connect status, existing Ando mapping, and
sync eligibility where returned. Mark unknown eligibility explicitly. Do not
substitute Ando's conversation list for Slack's available channel directory.
For a bounded search, state that scope instead of claiming a complete inventory.

Recommend a small public channel from the returned options and let the user
select it unless they already chose. Read the target channel's visibility and
membership before mapping private or Slack Connect content. Resolve identity
matches using supported identity evidence, not display names. Prepare the
source/target IDs, date coverage, file handling, unsupported content, and exact
write scope before execution. Do not enable every channel from a general request
to move a Slack workspace.

Native sync's documented coverage includes accessible public/private and Slack
Connect channels, excludes selectable DMs, and offers up to 180 days of backfill.
Verify current limits, private consent scopes, and the viewing member's own Slack
access before promising coverage. For older history or DMs, separately assess an
authorized export or source API and a supported history importer. History-import
endpoints alone do not establish a Slack adapter or an ongoing bridge.

## Execute and read back through the supported route

Use the discovered MCP operation or documented API for the approved pilot.
Reuse existing mappings, import IDs, source IDs, and supported idempotency keys.
If the service requires human consent or review, prepare the bounded request
first and return its actual approval URL. Continue from its status after approval;
do not restart discovery or ask for the same authorization again.

Read progress and per-channel outcomes through supported operations. Distinguish
accepted/queued, running, completed, skipped, and failed work. Resume from the
existing checkpoint and reconcile uncertain writes before retrying. If the API
has no progress operation, say so; do not invent a status endpoint or treat a
successful request as proof of complete history.

Compare source coverage with imported, skipped, and failed records. Read back
representative messages, authors, timestamps, roots/replies, source links,
reactions, and file accessibility where supported. Verify ongoing delivery
separately from historical coverage using an observed event or an explicitly
authorized test message. Outbound posting requires separate capability and sender
connection checks; inbound sync does not prove two-way fidelity.

## Handle a missing operation precisely

After checking available MCP and documented API routes, name the exact gap
and preserve the prepared selection/mapping. Reconnecting fixes identity or
grants; it does not add an unimplemented migration operation. A skill update
does not install new tools or deploy backend support.

Offer the smallest supported handoff: a returned consent/review link, or
**Settings → Slack sync** for only the operation unavailable through tools.
If the user requested plugin-only execution, report that operation as blocked
and explain the required API/MCP addition. Do not repeatedly open a browser,
request screenshots, or send the user through sign-in as a substitute for a
missing tool. Finish available reads and planning without claiming an import.

Keep source Slack available until coverage and cutover are accepted. Disconnecting
sync retains imported messages and paused mappings; it is not an undo. Return a
concise receipt with the verified source/target, selected mappings, counts,
history coverage, live-sync evidence, gaps, and next action.
