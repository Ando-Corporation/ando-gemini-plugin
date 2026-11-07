---
name: ando-give-agent-your-setup
description: Give an Ando agent the coding setup you use here, with the same repository, setup script and secret names, so teammates can ask it for code changes. Use when a user wants their Ando agent to work like their coding agent.
---

# Give an Ando agent your setup

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-give-agent-your-setup"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. This needs the person's own Ando connection and the `propose_agent_environment` tool; an agent's connection can't use it, and only workspace owners and admins can. If the tool is missing, say which of those is the reason rather than guessing.

This copies the person's setup into the Ando agent's cloud sandbox, so the agent can work while their computer is off. It is not the only option. If they want the agent to work on their own computer, with their checkout and their sign-ins, or they use a Claude Pro or Max plan, which can't run agents in Ando's cloud, they connect Claude Code or Codex as a local connection instead: **Studio → Apps → Claude Code** or **Codex**, **Let agents run tasks in Claude Code**, with Ando's desktop app on that computer. See [Connect your apps](../ando-connect-apps/SKILL.md). Both can be true at once.

Two things are called a machine in Ando: the agent's cloud sandbox in its **Environment** settings (where **End machine** stops that sandbox), and the person's computer under **Apps → Machines**. Say which one you mean.

## Gather the setup from this session

Work from what this session can see, and show the person what you found before writing anything:

- **Repository**: the GitHub repository you are working in, as owner/name, from `git remote get-url origin`.
- **Setup script**: the script that prepares this environment. In Claude Code on the web, the environment's setup script is what the person configured for this cloud environment; ask them to paste it when you can't read it. Otherwise build one from the repository's own install steps (its README, lockfile and package manager). The Ando agent runs it once per sandbox from the directory that holds each repository, so it starts by changing into the repository's folder.
- **Secret names**: the environment variables the setup and the code read that hold credentials, such as `STRIPE_SECRET_KEY`. Names only. Never read, print, or send a secret's value, and never ask the person for one.

The repository's CLAUDE.md, AGENTS.md and skills travel with the repository; the Ando agent reads them itself, so nothing needs copying.

## Propose it for the agent

Confirm the target agent with the person when the workspace has more than one Ando agent. Then call `propose_agent_environment` with the repository, setup script and secret names. Nothing changes on the agent yet: the tool saves a proposal that a workspace owner or admin reviews and approves in Ando. Approving adds the repository and secrets to what the agent already has and replaces its setup script. A newer proposal replaces one still waiting.

## Send them to approve it

Tell the person nothing has changed until they approve, and link `review_url`, where they check the script and secrets before approving. Then read the rest of the result back in plain words:

- A repository marked `not_connected` needs a workspace admin to connect it through Ando's GitHub app first.
- Each name in `secrets.missing` is a secret the workspace doesn't have yet. The person adds it themselves in Ando under Studio → Scripts → Secrets, bound to the hosts it is used with; then propose again with the same names.
- `secrets.unusable` names secrets an agent can't hold, with the reason.

Once approved, the setup script runs the next time the agent's sandbox starts, and its result shows on the agent's **Environment** page. If it fails there, that says nothing about a local connection, which runs on the owner's computer with the owner's own setup. Suggest the person ask the agent for a small change in a channel to verify it, and use [Troubleshoot your setup](../ando-troubleshoot-setup/SKILL.md) if it fails.
