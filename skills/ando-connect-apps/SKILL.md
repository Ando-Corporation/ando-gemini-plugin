---
name: ando-connect-apps
description: Connect the apps an Ando team uses, choose account ownership and agent access, and verify permissions with a real read. Use for first app setup, adding a required account, or connecting Claude Code or Codex on someone's computer so Ando agents can run tasks there.
---

# Connect your apps

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-connect-apps"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Establish the app, account, resource, and team outcome the connection should support. Connect the apps needed for that outcome rather than everything available.

## Choose ownership and access

Follow the current [Connecting apps guide](https://docs.ando.so/docs/connecting-apps-to-ando). Choose who owns each connection, who may use it, and which resources and actions it should allow. Workspace and personal connections belong to different audiences; an app can have both.

For a personal connection, determine whether an enabled agent should use the account only for the owner's requests or for others allowed to ask. Keep the supported restrictive default unless broader use is explicitly intended. A personal connection acts through its owner's provider account; Ando agent identity does not change that provider identity.

## Discover and connect

Read current connection state and reuse the intended account when already connected. Verify its actual provider/account identity; connection labels alone are insufficient. Use the exposed discovery path and exact schemas: compact catalogs may expose `list_connections`, `search_app_tools`, and `get_app_tool_schemas` (older compact servers use `list_app_tools`), while legacy catalogs may use `list_tools` and `get_tool_schemas`.

Use the returned supported connection or reconnect flow within the user's scope. If consent or administration requires the owner, prepare the configuration and provide the exact handoff. Never collect provider passwords, tokens, or verification codes in chat. See [optional capabilities](../../references/optional-capabilities.md) for shared app-tool boundaries.

Grant the intended members or agent only the required account, resources, and actions through supported controls. A successful OAuth callback does not establish that the agent can use the requested provider resource.

## Claude Code or Codex on a computer

A person's Claude Code or Codex is an app connection, not an agent: a local connection, made in **Studio → Apps → Claude Code** or **Codex** (or the sidebar's **Apps** section) with Ando's desktop app on that computer (macOS, Linux, or Windows: Codex with Git for Windows, Claude Code inside WSL2). An always-on server with no desktop app connects with `npx -y @andocorp/cli up` in the repository instead (**Connect a server instead** in Studio prints the command; Linux servers run Claude Code only). You cannot create or change it from this chat; prepare the handoff and send the person to the desktop app.

The connect dialog has two separate rows that can both be on. **Use Ando in Claude Code** (Get the plugin) is this plugin: the person using Ando from their session as themselves. **Let agents run tasks in Claude Code** connects the computer as a machine that the agents they pick can hand tasks to. Only the second row gives agents a place to work.

When they connect, they name the machine, choose the folders sessions may work in, and pick which Ando agents may start tasks there. Later they change both from the machine's page in **Studio → Apps** or from an agent's **Apps → Machines** tab in **Settings → Members → Agents**. The policy is per machine and applies to every granted agent:

- Allowed by default: reading and editing files in the chosen folders, running commands and tests, pushing branches, and installing packages or using the network.
- Never by default: merging, deploying, or pushing to main. Ando refuses those commands inside the task, but while pushing is allowed the task holds the owner's GitHub sign-in, which can still reach GitHub's API, so only branch protection on the repository holds main completely; say so when the owner asks what the policy guarantees.
- Never available: files outside the chosen folders, the owner's credential stores (SSH keys, keychain, gh settings, other tools' sign-ins), and the owner's own sessions. The GitHub sign-in given to tasks while pushing is allowed is the one deliberate exception.

Pushes and pull requests use the owner's GitHub sign-in on that computer.

An agent reads the connection back in `list_connections` as a row whose `server_url` starts with `local-tool://`, with provider `claude_code` or `codex`. It reaches it with `run_local_task` in the compact catalog, or with `execute_tools` and that machine's `run_task` in the legacy (flat) catalog; `get_app_tool_schemas` with `run_task` lists its folders. A person's own connection (this plugin) does not get those tools. See [optional capabilities](../../references/optional-capabilities.md) for how an agent hands over a task.

Tasks run only while the computer is awake with the desktop app open on that workspace and signed in, or while a server's connector is up. Verify with one small task from a granted agent in a thread and read back the notice it posts there; a machine that shows as online is not yet proof an agent can use it.

## Verify practical access

From the intended acting identity, perform a narrow read of a known permitted resource using its current tool schema. Confirm the provider account, returned resource, and relevant access. A read verifies reading only; do not use a provider mutation as a permission test without authorization.

Report the configured account, audience, verified operation, and remaining permissions or consent. Record where to disable access. For inbound provider events use [Set up an alert channel](../ando-alert-channel/SKILL.md); connecting an account alone does not create an alert feed or ongoing agent worker.
