---
name: ando-build-mini-app
description: Build or refine an Ando miniapp for a team workflow, such as triage, structured input, or a shared interactive view. Use to design, validate, and publish a miniapp to the intended audience.
---

# Build a mini app

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-build-mini-app"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Discover only tools needed for this request.

Clarify the user task, intended audience, input data, primary action, and desired output. Reuse answers already given. Determine whether the user wants a draft, published release, installed instance, or a working interaction in a conversation; these are distinct outcomes.

Inspect current miniapp discovery, read, update, and publication schemas. Read existing apps and instances before deciding to create another. Reuse a suitable app or instance when it serves the task. Inspect command/instance contracts, revisions, permissions, and current supported rendering surfaces. Do not invent command names, unsupported fields, or a revision argument on a legacy command read that does not accept it.

Human members and agents can publish miniapps and post command-backed instances when their current catalog exposes the miniapps capability and their grants permit the operation. Publication uses the authenticated membership as publisher and requires `miniapps:write` plus workspace write access; do not switch to an agent identity to publish for a human. Individual commands may still have narrower eligibility requirements.

To post an instance, read the command contract first, then use the advertised message tool's `miniapp_command` field with its name, revision, serialized inputs, and an `idempotency_key`. In the compact catalog, these fields belong inside `send_message.request`, alongside `kind: "conversation"`, `"thread"`, or `"direct"` and the corresponding destination. Follow the actual schema rather than assuming a legacy top-level shape. A request for a tracker or table needs a native interactive instance, not a prose list or a link to local HTML. Verify the resulting instance and persisted records. Displaying a DRI's name is not a notification; when tags are requested, resolve actual member IDs and use the supported mention contract.

If the host omits `miniapp_command` or publication tools, refresh discovery and check the authenticated identity, catalog version, and granted scopes. Current compact catalogs do not require a separate client capability opt-in for miniapps. Older catalogs may require a connection or host refresh after a server update. Report the precise missing capability instead of claiming humans cannot use miniapps or silently substituting a text message.

For connected data, verify the actual account and grants independently of Ando identity. Use current app-tool discovery to obtain provider schemas before execution. Keep secrets in supported storage and respect the audience of source data. An app must not expose personal provider data to a broader group.

Build the smallest useful interaction with a clear primary action and visible loading, empty, error, and permission states. Follow [Ando's current miniapp guide](https://docs.ando.so/docs/ando-mcp/publish-miniapps) and supported contracts. Read-only queries must remain read-only. Controls that change Ando or a provider need supported authorization/confirmation behavior and clear consequences. Do not use decorative buttons or invented handlers as proof of functionality. When an agent posts the card and a press asks that agent to act (approve, claim, hand off), mark that storage action `wakeAgent: true` so each accepted press wakes it (a write operation marked `wakeAgent: true` wakes it once the write has run, with its outcome); leave it off for votes, counters and edits that need nothing from the agent.

Validate the implementation using the available miniapp validation/preview path and representative permitted data. Verify meaningful actions against their persisted results. A mocked preview establishes presentation only. Handle stale revisions and denied access with an actionable recovery path.

Publish only within the user's authorized scope and intended audience. Verify the published revision, then installation and provider grants separately when requested. Read back the instance and test its intended interaction when authorized. Report which of draft, validated, published, installed, and verified working is actually complete, with links and remaining setup.

See [optional capabilities](../../references/optional-capabilities.md). For provider alert feeds use [Set up an alert channel](../ando-alert-channel/SKILL.md); for ongoing agent execution use [Set up a looped channel](../ando-looped-channel/SKILL.md). A published miniapp alone creates neither.
