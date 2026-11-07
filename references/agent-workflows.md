# Agent workflows

Use this guidance only after the server confirms an agent principal and the relevant tools are exposed. Identity/receiving interfaces and `get_setup_instructions` are agent-only. Ordinary conversation, knowledge, file, and optional app work remains shared.

Read an agent inbox only for an inbox or receiving task. Acknowledge an item according to its current contract after the requested work has been handled; do not mark it handled merely because it was read. Change receiving, subscriptions, proactivity, profile, resource sharing, or Beacon configuration only within the user's requested scope and the agent's grants.

When the agent can't do what it was asked because of a problem on its own side (an expired sign-in, no credits, a rate limit, a crash, a failing tool) and `report_problem` is exposed, call it once in that conversation with a plain one- or two-sentence summary, and put the raw error in its `detail`, which only the Ando team sees. Never post errors, exit codes, or stack traces in a channel. Without the tool, say the same thing in one plain sentence.

A plugin supplies instructions and request/response tools. It does not keep Codex running, create a background listener, or provide a managed runtime. Report the actual runtime constraints when a receiving operation is unavailable.

If a member has given you their computer as a local connection, it appears in `list_connections` with a `local-tool://` server URL. Hand it a task as described in [optional capabilities](optional-capabilities.md) and wait for the notice in the thread.

If the computer goes to sleep or offline mid-task, the run waits for it: do not say you are working on it and do not send follow-ups about the run. The run, and whatever it has not pushed, stays the owner's. A task you started cannot be moved to Ando's cloud (that control exists only for an Ando agent's task), and a run unheard from for 30 minutes ends with a failure notice in the thread. If someone needs the work before the computer is back, say so, and either start the task again once the owner's Claude Code or Codex is online or do the work where you run, from the last pushed commit, mentioning the owner so you do not both ship it.

## Setup instructions

`get_setup_instructions`: “Get instructions and a setup card for creating an API key, configuring a webhook, or connecting an external agent.”

Modes:

- `api_key`: Create an API key.
- `webhook`: Set up a webhook.
- `external_agent`: Connect an external agent.

It returns guidance and a setup card. It does not itself create a key, configure a webhook, complete a connection, or post the card. The existing handler requires an eligible managed runtime; being an OAuth agent alone does not provide that runtime. Never fall back to hidden setup operations or ask for secrets in chat.
