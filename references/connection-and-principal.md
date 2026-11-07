# Connection, workspace and identity

Use `get_workspace_overview` for current identity when offered. For an explicit switch, use `get_reconnection_instructions` when offered and follow the supported host OAuth handoff. After reconnecting, read the overview again and compare the returned workspace and principal to the requested target. Older catalogs may instead offer `manage_connection`; its compatibility workflow is below. Third-party account setup uses `connect_app_account` when offered and is separate from Ando identity.

The server is `https://mcp.ando.so/mcp`. The host manages OAuth and credentials. No API key, token, client ID, guessed scope or catalog override is bundled.

Use `manage_connection` when available. It is distinct from `manage_connections`, which manages third-party app integrations. Discover the actual schema; do not assume a newer tool exists on an older deployment.

- `request: {kind: "current"}` reads the actual workspace and acting identity.
- `request: {kind: "list"}` explains how to reach the signed-in OAuth chooser. It does not enumerate another person's workspaces through a workspace-scoped or agent credential.
- `request: {kind: "switch"}` requests that chooser when the user has not selected an exact target. If trusted IDs are already known, include `target` with `workspace_id`, `principal_type` (`user` or `agent`) and the exact `workspace_membership_id` for an agent. Targets are intent, never permission.

## Bring the user to the chooser

On an explicit switch request, call `manage_connection` and follow its handoff. Use the host's supported OAuth login/reconnect action for the exact Ando connection. Present the fresh authorization URL produced by that action: it brings the signed-in person to Ando's existing workspace-and-identity approval flow. Do not fabricate a link to `/oauth/consent`: it needs the host's fresh OAuth state, PKCE and callback. Never collect passwords, keys, tokens or verification codes in chat.

The authenticated person chooses a workspace from their verified memberships, then continues as themselves or selects an eligible existing agent. Email equality is not identity or authorization evidence. An agent's token does not grant the installer’s account-level access.

A local connection (Claude Code or Codex on someone's computer that Ando agents run tasks on) is not an identity on this chooser. It is an app connection made from Ando's desktop app, or with `ando up` on a server, in **Studio → Apps**, and switching the identity of this chat neither creates nor changes it. For Claude Code, Codex and Claude or ChatGPT chats, **Connect as me** is the normal choice; **Add as an agent** is for other runtimes and for Claude Code or Codex agents that already exist.

Hosts without a bundled helper, including Claude.ai, Claude Code and other MCP clients, reconnect from their own connector settings for the Ando connection: the connector's reconnect or sign-in control, or `/mcp` in Claude Code. That control opens Ando's chooser, where the signed-in person selects the workspace and identity. Then verify as below.

## Verify before continuing

1. After approval, refresh discovery for the exact connection and loaded chat. Supported host integrations can call `config/mcpServer/reload`, then inspect `mcpServerStatus/list` for that thread. Confirm the human/agent tools actually changed as expected. A successful browser callback alone is insufficient.
2. Call `manage_connection` with `request.kind: "current"` and `expected` containing the target workspace and identity. Require `verification.matches_target: true`. Credential propagation can lag the callback: if the old principal is still returned, pause and make one bounded follow-up read before reporting the switch as pending. Do not loop or start another OAuth flow automatically. If the person chose in the browser without known IDs, first report the returned workspace/name/type and confirm it is their intended choice; do not infer it from “same email.”
3. Discard old schemas, permissions, destinations and prepared writes. Re-resolve channels in the selected workspace. Reconcile pending writes; do not automatically replay a write approved for another author or workspace.
4. If the tool is absent, use the current exposed identity read and compare the same fields. If refresh is unavailable during the current turn, report verification pending and ask the user to continue in this same chat. On the next turn, repeat identity and callable-tool checks before any writes. If still stale, use the host connection refresh control. Reopening or starting a chat is a last-resort recovery, not the normal switch flow; a new chat alone never switches credentials.

API keys have fixed identity/workspace. Use the host's secure settings to replace a key or connect with OAuth; `manage_connection` cannot transform a key into another identity.

Catalog policy is server-controlled. Installing this package does not migrate an existing client. Use the current exposed catalog; never add principal/profile headers. A switch may affect other chats sharing that connection, so each must reverify before writing.

## Callable-tool check

Check the tools actually offered to this chat, not just the helper's `runtime_tools` or the server's overview. Any offered tool absent from the new principal's advertised capabilities is stale or otherwise inconsistent: keep the switch pending. A human catalog must not retain agent inbox, receiving or profile tools. For an agent, check the expected granted agent tools. Check the current schema before use, since a matching name does not prove schema freshness.

Hosts may intentionally filter or defer tools. A missing advertised tool alone does not establish a stale cache; discover it through the supported host mechanism if available. If a tool needed for the next task is unavailable, report that separately and do not guess its arguments or bypass the host.

## Diagnose versions and missing tools

The plugin package version, catalog version and deployed MCP revision are separate.
Read the overview with `include_capabilities: true`, and when it offers them,
report `server_revision`, `catalog_fingerprint`, `supported_modes` and
unavailable-capability reasons. A null revision is unknown.
The overview describes the server-authorized catalog; compare it with this host's
actual full tool declarations before claiming that a tool is callable. A plugin
update does not change the author, grant capabilities or deploy the server.
