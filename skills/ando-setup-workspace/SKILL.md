---
name: ando-setup-workspace
description: Create or improve an Ando workspace's starting structure and invite the intended team. Use for new-team onboarding, initial channels, and workspace setup.
---

# Set up your workspace

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-setup-workspace"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Help the team reach a useful starting state, then verify what exists.

## Choose the starting structure

Establish the team's purpose, existing workspace, intended members, and first shared activity from context. Reuse an existing workspace and channels when they fit. Ask only about missing choices that affect creation, invitations, or access. A request for a suggested structure does not authorize creating it or inviting people.

Start with the conversations the team needs for its first real activity. Give each a clear purpose and audience. Avoid a department-wide channel taxonomy, automatic app connections, and agent invitations unless the user requests them. Private work needs an explicitly chosen audience.

## Create and invite within scope

Inspect the current workspace and permitted discovery before making changes. Use current exposed tools for creation or invitations only when their schemas and the acting principal's grants support the operation. Do not guess administration endpoints or silently create a second workspace.

When setup requires the app, use [Start here](https://docs.ando.so/docs/start-guide) and the current workspace invite surface for the exact remaining steps. Prepare the names, purposes, audience, and invitation list first. Keep invite links in their intended audience; never request passwords or credentials in chat.

Invite only the people the user authorized. Resolve ambiguous people or addresses before sending. Read existing membership and pending invitations where available to avoid duplicate invites. Distinguish an invitation sent from a person actually joining.

## Verify the starting state

Read back the workspace, created conversations, visibility, and memberships or invitation status. Report what is ready, who still needs to accept, and where the team should begin. For the next step, use [Connect your first agent](../ando-connect-first-agent/SKILL.md), [Connect your apps](../ando-connect-apps/SKILL.md) when the team wants their Ando agents to code in Claude Code or Codex on their own computers, or [Run your first team workflow](../ando-first-team-workflow/SKILL.md) when requested. Do not claim the team is active from configuration alone.
