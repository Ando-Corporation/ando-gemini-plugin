---
name: ando-troubleshoot-setup
description: Diagnose missing Ando access, silent agents, failed notifications, or broken app and MCP connections. Use when setup exists but the intended workflow does not work.
---

# Troubleshoot your setup

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-troubleshoot-setup"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Start with the failed action, intended workspace/identity/resource, client or runtime, exact error, and when it last worked. Ask for missing facts only when they affect the next diagnostic step; never ask for secrets.

## Find the failing boundary

Use narrow read-only checks and current schemas before proposing configuration changes. Distinguish:

- Wrong workspace or identity: read the actual principal and workspace, then compare the intended target. Use [connection guidance](../../references/connection-and-principal.md) for an authorized switch and verify the calling client's refreshed tools afterward.
- Missing conversation or app access: verify membership, visibility, selected provider account, and granted resource/tool access. Do not join a private conversation, broaden grants, or bypass denial merely to inspect it. Use [Connect your apps](../ando-connect-apps/SKILL.md) for an authorized account repair.
- Broken authentication or stale tools: inspect the host's actual offered schemas and supported reconnect/refresh controls. A successful callback or plugin update does not prove that this chat has new credentials and tools. Use the host-specific recovery in the connection reference rather than assuming every client needs a new chat.
- Silent agent: verify agent identity, intended conversation membership, receiving route, running worker/session, and relevant response scope. Trace the test message and any available receiving/run status. MCP access alone does not wake an agent. Load [agent workflows](../../references/agent-workflows.md) only for an authenticated agent and a receiving task; do not sweep an inbox as a generic diagnostic.
- Missing human notification: check Ando event/device preferences and browser/device permission separately. Use [Tune notifications](../ando-tune-notifications/SKILL.md); notification preferences are not agent receiving settings.
- Task on someone's computer not starting, paused, or never reported: the agent's message in the thread shows the run, and the Claude Code or Codex app feed in **Studio → Apps** logs runs. Do not disconnect or reconnect the machine as a first step; it is the owner's.
  - Queued: the computer or the desktop app is off (on a server, the connector is stopped; `ando status` shows it), or the workspace is not the one open in it. The owner opens the desktop app on that workspace, or starts the connector again; a task nobody takes within 24 hours ends on its own.
  - Refused before it queues: Claude Code or Codex itself is signed out on that computer (`machine_signed_out`). Signing in to Ando does not help; the owner signs in to Claude Code or Codex there (`claude auth login`, or `codex login`), and the agent starts the task again.
  - Paused: the computer has been asleep or offline for about five minutes. For an Ando agent's task, only the owner can continue it elsewhere, with **Move to Ando's cloud** on the agent's message; a task a connected agent started has no such control and resumes when the computer is back. Either way a run unheard from for 30 minutes is ended with a failure notice.
  - Refused: the agent has no machine granted (the owner grants it on the machine's page in **Studio → Apps** or in the agent's **Apps → Machines** tab), the folder is not one the owner allowed (`get_app_tool_schemas` with `run_task` lists them), or the task asked for something the owner's policy never allows, such as merging or pushing to main.
  - Finished: a finished task posts a notice in the thread and wakes the agent. If the notice is there and the agent said nothing, treat it as a silent agent, not a machine problem.

If evidence is inaccessible, identify the responsible owner and smallest check they can perform. Distinguish observed failure from an inferred cause.

## Repair and repeat the original action

Apply a reversible repair within the user's authorized scope, retaining prior configuration where needed to undo it. Reuse existing identities and connections. Do not rotate a credential, replace an agent, cancel an invitation, disable a workflow, or recreate configuration as a speculative first step; name the concrete need and effect before such a repair.

On denied access, explain the missing grant and stop that path. On a dependency failure, retry once after a brief pause; do not loop. For uncertain writes, read back the result before retrying.

Repeat the original bounded failing action after repair. For a silent agent, verify a real response in the intended conversation; for an app, verify the requested permitted operation. Report the evidence, what changed, how to undo it, and any remaining owner action. Mark an untested repair as pending verification.
