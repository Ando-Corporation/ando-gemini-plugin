---
name: ando-connect-first-agent
description: Connect a first Ando agent, choose its conversation and app access, and verify a real exchange. Use for agent onboarding or a first working connection with an external runtime such as Grok Bot, Hermes or a custom harness. For Claude Code or Codex, first decide whether the person wants their agents to code on their computer (a local connection, see Connect your apps), to use Ando from their own session (this plugin, connect as me), or an always-on coding agent (an Ando agent).
---

# Connect your first agent

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-connect-first-agent"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Establish which agent, client/runtime, workspace, and first conversation the user intends. Reuse an existing agent identity when reconnecting.

## Connect through the supported path

Follow the current [Invite your agents guide](https://docs.ando.so/docs/external-agents) for the chosen client. Claude Code and Codex relate to Ando in three different ways, and only one of them is an agent. Settle which the person means before anything else.

- If they want their Ando agents to code on their computer, with their own plan and files, that is a local connection: an app connection, not an agent, made from Ando's desktop app on that computer (macOS, Linux, or Windows with WSL2 for Claude Code), or with `ando up` on an always-on server, in **Studio → Apps → Claude Code** or **Codex**, under **Let agents run tasks in Claude Code**. Use [Connect your apps](../ando-connect-apps/SKILL.md) for it; nothing in this chat can create it.
- If they want to use Ando from their own Claude Code, Codex or ChatGPT session, that is this plugin, with **Connect as me** on the approval screen.
- If they want an agent that codes while their computer is off, that is an Ando agent (**Add an agent → Ando agent**), which they can later give their setup with [Give an Ando agent your setup](../ando-give-agent-your-setup/SKILL.md). Claude Pro and Max plans can't run agents in Ando's cloud, so for those the local connection is the way to use that plan with Ando agents.

Ando no longer offers Claude Code or Codex as their own agents, and the **Add an agent** dialog points people to **Apps**. For a new connection, do not redeem an invite or choose **Add as an agent** for them, or for a Claude or ChatGPT chat. Claude Code and Codex agents that already joined as members keep working: reconnect them by picking the existing agent in the approval screen's **Agent** menu. The external-agent path below is for other runtimes: Grok Bot, Hermes, OpenClaw, Cursor, a custom harness. A plugin installation, a connected machine, or a fetched invite page does not prove an agent is connected or running.

Use exposed setup tools only within their current eligibility. [Agent workflows](../../references/agent-workflows.md) explains that setup instructions can require an eligible managed runtime and return guidance rather than completing setup. Otherwise prepare the exact app/client handoff. Store credentials through the runtime's supported secure settings, never in chat or the skill files.

Verify the actual workspace and agent membership through the connected runtime's current identity read. A human OAuth connection is not an agent connection. Use [connection guidance](../../references/connection-and-principal.md) for an identity change, then refresh the calling client's tools and verify again.

## Choose access and receiving

Add the agent only to the intended conversations using an authorized operation or the current app surface. Workspace membership alone does not provide access to every conversation. For apps, use [Connect your apps](../ando-connect-apps/SKILL.md) and grant only the required account and actions.

Check how this runtime receives work: an active session, a supported listener, a managed worker, or an explicitly configured scheduler. Do not infer ongoing participation from MCP access. A persistent channel workflow is a separate request; use [Set up a looped channel](../ando-looped-channel/SKILL.md) when needed.

## Verify a real exchange

Within the user's authorized test scope, send or have the user send one short request in the intended conversation. Confirm that the agent receives it and replies there as the expected agent with a useful answer. Read back the reply and link it. An introduction verifies author and delivery; a response to a subsequent request verifies a working exchange.

Report connection, access, receiving runtime, and exchange evidence separately. If the test needs user action or the agent stays silent, name the missing step and use [Troubleshoot your setup](../ando-troubleshoot-setup/SKILL.md). Do not report success from a setup card or OAuth callback alone.
