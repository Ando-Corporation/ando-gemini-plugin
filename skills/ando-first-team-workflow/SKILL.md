---
name: ando-first-team-workflow
description: Complete a first useful Ando team workflow with people and an agent, such as a decision, handoff, or task. Use when onboarding should produce a real shared outcome.
---

# Run your first team workflow

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-first-team-workflow"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Choose one real piece of work the team wants to finish, rather than a generic demonstration.

## Define a useful outcome

Resolve the conversation, participants, source context, and completion condition from the user's request. Offer a decision, handoff, or task only when no outcome is already specified. Keep the first workflow bounded enough to verify in the current session; do not create a recurring automation or broad project plan as an onboarding default.

Check that the intended agent and people can access the required conversation and sources. If an agent is not connected, use [Connect your first agent](../ando-connect-first-agent/SKILL.md) before depending on its response.

## Complete the chosen work

Read the relevant discussion and replies before acting. Follow the appropriate existing workflow:

- Decision: use [Find answers](../ando-find-answers/SKILL.md) to recover the evidence and distinguish proposals from agreed decisions. If the team has not decided, present the concrete choice to the authorized decision maker; do not invent consensus.
- Handoff: use [Coordinate work](../ando-coordinate-work/SKILL.md) to capture the requested outcome, agreed owner, source links, remaining dependencies, and next action. A sent handoff is distinct from its recipient accepting it. When the handoff is code work to an Ando agent, the agent may do it in its own sandbox or on a teammate's computer through a granted local connection; the thread shows which, and a task on a computer finishes only while that computer is awake. An agent reply that it has started a task is not the finished work; wait for the notice in the thread.
- Task: inspect existing tasks, reuse or create the authorized task through the current exposed contract, and record an actual owner and completion condition where agreed. Task creation is distinct from completing the underlying work.

Draft in chat when sending is not authorized. When authorized, return the result to the intended conversation or source thread with the actual author and source links. Preserve prior authorization without introducing repeated confirmation steps.

## Verify the outcome

Read back saved messages, decisions, or task state as appropriate. Compare the evidence to the completion condition. An agent reply alone does not establish an agreed decision, accepted handoff, or finished assignment.

Report what the team accomplished, link the resulting discussion or task, and identify the next owner or unresolved choice only where supported. Keep any reusable process or shared-context update optional unless requested.
