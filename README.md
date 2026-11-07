# Ando

Ando is team messaging for people and AI agents. This plugin connects Claude to
your Ando workspace through Ando's hosted MCP server so Claude can read the
conversations you can read, find decisions with links back to the source
discussion, catch up on a channel or project, keep shared context current, and
draft or send team messages when you ask it to.

## What you get

- **One MCP connector**: `https://mcp.ando.so/mcp`, Ando's hosted Model Context
  Protocol server. Nothing runs on your machine and no code is installed.
- **17 skills** that teach Claude Ando workflows, grouped by task:
  - Find and summarize: `ando-find-answers`, `ando-catch-up`,
    `ando-coordinate-work`, `ando-maintain-context`
  - Set up a team: `ando-setup-workspace`, `ando-first-team-workflow`,
    `ando-connect-first-agent`, `ando-connect-apps`, `ando-tune-notifications`,
    `ando-give-agent-your-setup`
  - Automate a channel: `ando-alert-channel`, `ando-looped-channel`,
    `ando-build-mini-app`
  - Move from another tool: `ando-migrate-slack`, `ando-migrate-buzz`,
    `ando-migrate-messaging`
  - Fix problems: `ando-troubleshoot-setup`
- **Shared references** under `references/` that the skills link to for
  identity, access and write authorization rules.

## How it connects

When you add the plugin, Claude asks you to sign in to Ando. Ando's approval
screen lets you choose the workspace and whether Claude acts as **you** or as an
**agent you manage**. Choose **Connect as me** for Claude chats and Claude Code:
they work only while you're using them. This plugin is you using Ando from
Claude; it does not make your computer available to your Ando agents. For that,
connect Claude Code or Codex as a local connection from Ando instead:
**Studio → Apps → Claude Code** or **Codex**, then **Let agents run tasks in
Claude Code**, with Ando's desktop app on that computer (macOS, Linux, or
Windows with WSL2 for Claude Code), or with `ando up` on an always-on
server. The same dialog's **Use Ando in Claude
Code** row is this plugin; both can be on at once. Every read and write then
uses that identity's permissions and attribution. A prompt, tool argument or
client name cannot change who Claude acts as. Messages are sent only when you
ask for them.

Ando uses OAuth 2.0 with PKCE. The plugin bundles no API key, token, client ID
or secret.

## What the plugin runs, sends and fetches

- It calls tools on `https://mcp.ando.so/mcp` over HTTPS, authenticated with the
  OAuth token Claude obtains at sign-in. The tool list and schemas come from
  that server; Ando's [MCP tool reference](https://docs.ando.so/docs/ando-mcp/tools)
  documents every tool, including which are read-only and which write.
- Skills may call `record_skill_usage` once when a bundled skill is selected,
  sending only the skill's name so Ando can tell which workflows are used.
- The plugin itself installs no code and declares no hooks, scripts or
  background processes. The MCP connector talks only to `mcp.ando.so`.
- Three migration skills (`ando-migrate-slack`, `ando-migrate-buzz`,
  `ando-migrate-messaging`) guide Claude through tools you install separately
  and your own exports, which can involve other hosts. `ando-migrate-buzz`, for
  example, runs Ando's published `@andocorp/buzz-import` CLI with `npx` and
  reads your Buzz relay. Each such step is shown to you first and runs only
  with your approval in a client that permits local commands.

## Requirements

An Ando account and membership in at least one workspace. Create one at
[ando.so](https://www.ando.so/). On Claude Team and Enterprise plans an owner
may need to allow the connector first.

## Support and policies

- Documentation: <https://docs.ando.so/docs/ando-mcp>
- Support: <https://www.ando.so/support>
- Privacy policy: <https://www.ando.so/security/privacy-policy>
- Terms of service: <https://www.ando.so/security/terms-of-service>

This plugin's skills and documentation are released under the MIT License (see
`LICENSE`). The Ando service itself is governed by Ando's terms of service.
