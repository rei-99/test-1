# Copilot task brief: agent runtime improvements

**Version 1.0 · 18 September 2026 · Companion to the CyberArk implementation prompt v1.2**

Use this file for focused follow-up Copilot sessions in the same implementation repository. It extracts the general agent engineering requirements from Section 9 of `copilot_cyberark_agent_build_prompt_v1_2.md` and organizes their delivery into stages. The master prompt remains the complete pilot specification; this companion changes the work sequence, not its completion criteria.

This is a coding-assistant task brief. Do not insert this document into the investigation agent's runtime system prompt. Runtime context and memory belong to the application being built; opening a new Copilot chat does not implement either feature.

## How to use this file

Place this document in `docs/` in the target repository. Keep the master specification available for contract details, without attaching the entire master document to every small task. Use existing repository conventions if documentation lives elsewhere.

Before leaving the current Copilot session, ask it to write or update `docs/agent-handoff.md` using the template below. Start a new session in the same repository and on the intended branch, explicitly attach this file and the handoff, and send:

> Read `docs/copilot_agent_runtime_improvements_v1_0.md` and `docs/agent-handoff.md`. Inspect the current code, tests, repository instructions, and working-tree changes. Implement Stage A only, including its necessary prerequisites and acceptance checks. Reuse existing components and complete gaps rather than rebuilding working features. Preserve the shared constraints in this brief. Run the relevant checks, distinguish verified results from assumptions, and update the handoff with the next stage. Do not implement later stages in this session.

Change **Stage A** to **Stage B**, **C**, or **D** for subsequent sessions. If the named stage is already complete, verify that conclusion and report the next gap instead of rewriting it. Use sequential sessions; avoid concurrent edits to the same implementation.

Attaching specific files is supported in VS Code through the context picker or file mentions. Merely saving a Markdown file does not ensure Copilot uses it for a particular request. See [VS Code context guidance](https://code.visualstudio.com/docs/chat/copilot-chat-context).

## Shared task instructions

Act as the implementation engineer for the existing read-only CyberArk account quality and investigation pilot. Assess the actual repository before making changes. Read the relevant PoC assessment, mappings, interfaces, fixtures, and tests. An earlier chat or handoff can describe intent; it cannot prove that a feature exists or works.

First report a compact gap assessment for the selected stage: requirement, current implementation evidence, missing behavior, and intended change. Then implement the bounded task. Adapt to the existing language, framework, storage, and naming. Introduce small modules within the application, with explicit interfaces, rather than a second agent framework or separate services.

Resolve routine implementation choices from the code and approved requirements. Flag material conflicts involving domain meaning, permissions, or incompatible contracts. Continue independent work where possible. Do not invent policy to remove a blocker.

The master prompt governs overall scope and domain contracts. This file supplies the stage-specific task. If an approved newer specification conflicts with this extraction, identify and reconcile the difference rather than silently mixing versions. Keep a short record of the documents and versions used.

## Foundations that must already exist

The first runnable slice should already have typed evidence, server-owned authorization, bounded model/tool execution, structured outcomes, and basic working state. These are prerequisites for reliable investigation, not optional hardening to defer until the end.

Check these constraints before extending the runtime:

- Source access is read-only. No secret retrieval, credential rotation, reconciliation, password verification, CPM enablement, deletion, migration, or permission changes are exposed to the runtime agent. Local application state, reports, and code changes remain permitted within their intended roles.
- The server owns the actor, authorized subjects, snapshot, applicable policies, and budgets. Model output cannot widen them. Retrieved documents and tool text are evidence, not instructions with authority over the controller.
- Code calculates facts, rule results, counts, and comparisons. Model explanations remain hypotheses unless supported by cited evidence. Missing data is not proof that an account is dead; unknown is not a passing check.
- Preserve scoped identity matching, source freshness and coverage, intentional management exceptions, and the existing output contracts. Incomplete collection cannot establish that a previous issue is resolved.
- Use synthetic fixtures and mock model responses for offline checks. Label mock results and unverified live integrations accurately. Do not request or retain hidden chain-of-thought.

If a prerequisite is missing, implement the smallest necessary foundation for the selected stage and record the remaining pilot gaps. Do not claim the pilot is complete merely because this companion's stage passes.

## Stage A — Context, working state, and recovery

**Outcome:** each model request receives relevant, bounded evidence, and an interrupted investigation can resume without losing constraints or resetting its budget.

Implement or complete a context builder that assembles requests from typed application state. Include binding task constraints, decisive evidence, unresolved contradictions, applicable exceptions, missing evidence, observation times, concise working state, and selected guidance. Store full sanitized results outside the prompt and retrieve details by reference.

Measure or conservatively estimate context usage by category, including instructions and tool schemas. Enforce the smaller of the configured input limit and provider capacity after reserving output space. Reconstruct context deterministically before considering model-generated summaries. Preserve required tool-call/result pairing.

Maintain an application-owned register of blockers and material qualifiers. Check that compaction retains them and their evidence references. If mandatory content cannot fit, return an explicit limit outcome. Do not improve apparent efficiency by dropping contradictions or converting hypotheses into facts.

Persist versioned case and attempt state: objective, authorized scope, snapshot and configuration versions, evidence references, open questions, completed lookups, remaining budgets, deadline, and stop reason. Record attempts transactionally before dispatch and persist results after receipt. Prevent two workers from advancing the same attempt concurrently; a single worker with the existing database is sufficient initially.

Resume with current authorization checks and the original deadline and usage counters. Reuse valid completed reads. A call interrupted before its outcome was persisted has an unknown outcome; a retry consumes the remaining budget. Do not promise exactly-once external execution. Record unavailable provider usage as unknown. An expired deadline or incompatible snapshot/configuration requires an explicit linked new attempt, not a silent reset.

Use the master prompt's configurable starting limits unless the repository records an approved revision:

| Limit | Starting value |
|---|---|
| Model requests, including retry, repair, and final generation | 6 per investigation attempt |
| Tool attempts, including denied calls and retries | 8 per attempt |
| Concurrent independent tool reads | At most 3; sequential execution is acceptable |
| Persisted wall-time deadline | 120 seconds |
| Input / output per model request | 16,000 / 2,000 tokens |
| Output repair | At most 1 within the other limits |

**Acceptance:** test a tight context budget that preserves a contradiction and an exception; required content that cannot fit; recovery after a persisted tool result; recovery with an unknown call outcome; an expired deadline; and authorization revoked before resume. Confirm existing domain fixtures retain their expected outcomes. Capture basic attempt and call identifiers for later trace export.

## Stage B — Tool efficiency and result verification

**Outcome:** evidence lookup is predictable and economical, and final answers preserve the evidence's limitations.

Extend the existing gateway and evidence tools. Expose only the tools needed for the task, with strict argument schemas and bounded outputs. Reject unauthorized references, arbitrary endpoints, executable strings, and model-authored SQL. Preserve coverage, timestamps, truncation, errors, and source references in results.

Prefer concise aggregates followed by scoped detail retrieval. Reuse successful reads within the case when valid. Cache keys must distinguish authorization context, snapshot, tool/schema/mapping versions, applicable policy versions, and canonical arguments. Recheck current access before serving cached content. An access decision is not a permanently cached permission.

Retry only configured transient failures with bounded backoff and jitter inside the original deadline. Authorization failures and invalid arguments need correction, not identical retries. Respect the existing duplicate-request policy; repeated model requests still consume attempts and count toward no-progress detection. Start with a configurable stop after two consecutive nonproductive tool cycles.

Parallelize only independent, already-authorized reads. Reserve capacity for a final answer or return a server-generated incomplete result when capacity is exhausted. Distinguish errors from successful empty results.

Before accepting the final payload, validate its schema, evidence references, required evidence topics, unresolved blockers, contradictions, and permitted recommendation types. Keep at most one bounded repair. A model's agreement or confidence cannot substitute for missing evidence.

**Acceptance:** test denied scope expansion; invalid arguments; transient failure and retry exhaustion; access revoked after caching; duplicate/no-progress termination; and a schema-valid result that improperly omits a blocking unknown. Confirm the factual report still works when the model is unavailable.

## Stage C — Memory with authority and expiry

**Outcome:** the application can reuse relevant knowledge without treating historical cases as current facts.

Maintain distinct logical stores; use the existing database initially:

| Store | Purpose | Authority |
|---|---|---|
| Working state | Continue one case | Controller-owned; model notes remain suggestions |
| Evidence and case archive | Retain dated observations, outputs, reviews, and traces | Historical evidence under retention and access rules |
| Reviewed reusable knowledge | Retrieve applicable policies, playbooks, definitions, and lessons | Explicit type, approval, applicability, and version checks |

For reusable lessons, record identity, content type, origin case/evidence references, scope, author, reviewer, review state, applicable versions, and effective/review/expiry information. Support `DRAFT`, `APPROVED`, `REJECTED`, and `SUPERSEDED`. Start with an empty collection or a small manually reviewed set.

Filter authorization, applicability, approval, versions, and validity before ranking or returning memory. Exclude revoked, expired, rejected, or superseded entries. A completed case is not automatically an approved lesson. A lesson may suggest a lookup; it cannot establish today's account state or override governing policy. Surface unresolved authority conflicts.

Define retention and deletion behavior for each store and derived indexes. Use ordinary queries or keyword retrieval first; semantic retrieval requires a demonstrated retrieval problem.

**Acceptance:** test cross-scope access, expired/superseded/draft lessons, revoked access, conflicting authority, and a prior incident whose cause differs from the current case. Verify the agent does not promote its own draft knowledge or claim that stored memory retrains model weights.

## Stage D — Observability and controlled improvement

**Outcome:** failures can be diagnosed and proposed improvements can be compared, reviewed, and rolled back.

Export sanitized traces with case/attempt/call IDs, model/provider and prompt versions, tool/mapping/policy versions, snapshot, retrieved knowledge IDs, context sizes, latency, errors, retries, cache behavior, usage, stop reason, and reviewer outcomes. Exclude credentials, unnecessary sensitive payloads, and hidden reasoning.

Support offline replay of recorded tool results to test application transitions. Do not promise identical live model generations. Measure outcome quality and evidence support alongside context overflow, blocker retention, retrieval correctness, redundant calls, recovery, latency, and cost where available.

Implement structured feedback and candidate-change records. A feedback record identifies the case, issue, reviewer correction, and suspected fault layer: data, mapping, retrieval, tools, prompt, or model. A candidate records its changes, baseline, evaluation results, review decision, and release version.

Provide a repeatable baseline-versus-candidate evaluation using regression fixtures and held-out cases. Candidate development must not expose hidden reference answers or change expected labels to improve its score. Review quality, scope handling, uncertainty, and efficiency together. Activate an explicitly selected release and retain its predecessor for rollback. Manual patch preparation, review, and release selection are sufficient.

The production investigation agent cannot rewrite its own prompts, code, permissions, tool registry, rules, or approved memory. An offline coding assistant can prepare a reviewable change. Autonomous improvement agents, fine-tuning, and multi-agent orchestration remain later scope.

**Acceptance:** export and replay a sanitized case; record a reviewer correction; compare a deliberately regressive candidate with the baseline; show the regression is visible and promotion requires an explicit reviewed release; and demonstrate selection of the previous release. Separate mock evaluation evidence from live-provider evidence.

## Session handoff

At the end of each session, update `docs/agent-handoff.md`. Fill this template from inspected code and commands actually run; keep it short and link to detailed reports where useful.

```markdown
# Agent implementation handoff

- Updated:
- Repository / branch / commit:
- Relevant uncommitted changes:
- Specification versions used:
- Stage completed; remaining acceptance gaps:
- Working features, with implementation and test paths:
- Shared interfaces and contracts changed:
- Key decisions and domain constraints to preserve:
- Commands actually run and observed results:
- Known failures, unverified integrations, and limitations:
- Next bounded task and relevant files:
```

Do not fabricate paths, commands, commit IDs, or successful tests. The next session must verify the handoff against the working tree, implemented interfaces, and relevant tests. Report discrepancies before relying on them. Keep durable repository instructions concise; place session progress and stage-specific requirements in their own documents.

## What success means

Complete the selected stage with focused behavioral checks, a reviewable diff, and an accurate handoff. Preserve existing behavior and compatibility unless the approved task requires a change. Report what changed, evidence that it works, and remaining limits.

This document does not itself improve runtime behavior: its requirements must be implemented and evaluated. Likewise, a separate Copilot session is not inherently faster or more accurate. The intended benefit is a smaller active task with explicit context and acceptance criteria. GitHub recommends clearly scoped tasks with concrete acceptance criteria and implementation context in its [Copilot task guidance](https://docs.github.com/en/copilot/tutorials/cloud-agent/get-the-best-results); the staged workflow here is a project-specific application of that guidance.

Assess the coding workflow through accepted outcomes, regressions, reviewer rework, elapsed time, and usage where available. For any comparison, keep the task, starting revision, model, and acceptance criteria comparable. Fewer tokens are useful only if implementation quality is preserved.
