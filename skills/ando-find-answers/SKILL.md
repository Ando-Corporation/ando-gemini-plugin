---
name: ando-find-answers
description: Answer questions using Ando shared knowledge and conversations with source links. Use to find a decision, explain why it was made, locate evidence, or reconcile conflicting project information.
---

# Find answers

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-find-answers"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. Discover only tools needed for this request.

Identify the question and its relevant scope. Begin with the most relevant permitted knowledge or conversation search; use narrow queries and refine based on results. Do not assume all workspace content is visible.

Read source resources and relevant thread replies, preserving dates, authorship, audience, and revisions. Distinguish firsthand decisions from quotations, tentative suggestions, and summaries. When sources conflict, trace whether a later authoritative discussion superseded an earlier one. Explain unresolved contradictions rather than choosing the newest message automatically.

When exposed, answer_from_knowledge can return a cited synthesis. Inspect sources before making exact quotations or decisive claims. A no_answer response can lead to message search; an in_progress response requires bounded waiting, not repeated job creation. Use only the modes and fields in the current schema.

Lead with the answer and attach source links to supporting claims. Distinguish evidence from inference and name material access or coverage gaps. If no source settles the question, say what remains unknown and identify the smallest next clarification. Never fabricate a permalink, owner, date, or final decision.

See [shared workflows](../../references/shared-workflows.md). Use [Maintain shared context](../ando-maintain-context/SKILL.md) only when the user wants knowledge updated.
