Prepare this project for a new Copilot implementation session.

Read docs/copilot_agent_runtime_improvements_v1_0.md and inspect the current code, tests, repository instructions, and working-tree changes.

Create or update docs/agent-handoff.md with:
- Current branch, commit, and relevant uncommitted changes.
- What is actually implemented, with file and symbol references.
- Existing architecture, shared interfaces, and PoC components reused.
- Important decisions and domain constraints.
- Commands actually run and their observed results.
- Known failures, incomplete features, and unverified integrations.
- Readiness for Stage A and any missing prerequisites.

Distinguish implemented behavior from planned behavior. Verify claims against the repository rather than relying only on this conversation.

Keep the handoff concise and useful to a fresh session. For this task, change documentation only.







Implement Stage A of docs/copilot_agent_runtime_improvements_v1_0.md: context management, working state, and recovery.

First read the shared constraints, Stage A requirements, docs/agent-handoff.md, and applicable repository instructions. Inspect the relevant implementation and tests. Verify the handoff against the current working tree.

Give a brief gap assessment, then implement the missing behavior. Reuse existing components and follow the current stack and architecture.

Focus on:
- Building bounded model context from typed application state.
- Preserving evidence references, contradictions, exceptions, missing information, and observation times during context reduction.
- Accounting for input/output capacity and execution budgets.
- Persisting case state, attempt state, checkpoints, and call outcomes.
- Resuming with current authorization, original deadlines, and remaining budgets.
- Handling interrupted calls, expired attempts, and incompatible snapshots explicitly.

Preserve read-only source access, server-owned authorization, deterministic domain checks, and existing output contracts. Implement only necessary prerequisites and Stage A. Do not add later-stage features.

Run the Stage A acceptance checks and relevant existing regression tests. If a capability already works, verify and reuse it.

Update docs/agent-handoff.md with changes, actual validation results, remaining gaps, and the next bounded task. Finish with a concise implementation summary and any limitations. Do not stop after producing a plan.







Implement Stage B of docs/copilot_agent_runtime_improvements_v1_0.md: tool efficiency and result verification.

Read the shared constraints, Stage B requirements, docs/agent-handoff.md, and repository instructions. Inspect the actual gateway, tools, controller, validators, and tests. Verify Stage A prerequisites rather than assuming they are complete.

After a brief gap assessment, implement the missing Stage B behavior:
- Strict tool schemas, task-specific tool exposure, and server-enforced scope.
- Bounded evidence results with coverage, timestamps, truncation, and references.
- Valid per-case reuse of successful reads, with authorization rechecked.
- Bounded transient retries, deadline enforcement, and no-progress termination.
- Concurrency only for independent authorized reads.
- Final validation of evidence references, unresolved blockers, contradictions, and permitted recommendations.
- Explicit incomplete outcomes and continued factual reporting when the model is unavailable.

Preserve read-only source access, existing business semantics, interfaces, and budgets. Reuse working components. Keep errors distinct from successful empty results.

Implement Stage B and necessary prerequisites only. Run its acceptance checks and relevant regression tests, including revoked access after caching and a schema-valid answer that omits a blocking unknown.

Update docs/agent-handoff.md with implementation references, commands and results, remaining gaps, and Stage C readiness. Complete the implementation rather than stopping at recommendations.







Implement Stage C of docs/copilot_agent_runtime_improvements_v1_0.md: memory with explicit authority, scope, and lifetime.

Read the shared constraints, Stage C requirements, docs/agent-handoff.md, and repository instructions. Inspect existing persistence, retrieval, authorization, and knowledge models before introducing anything new.

After a brief gap assessment, implement:
- Distinct logical storage for current case state, historical evidence/cases, and reviewed reusable knowledge.
- Knowledge provenance, scope, review status, applicable versions, and validity dates.
- Authorization, approval, applicability, and freshness filtering before retrieval.
- Handling for revoked, expired, superseded, rejected, or conflicting knowledge.
- Retention and deletion behavior appropriate to the existing application.

Use the existing database and simple retrieval initially. Preserve read-only source access and the existing domain contracts.

Historical lessons may guide an investigation but cannot establish current account facts or override governing policy. The runtime agent must not approve its own lessons.

Implement Stage C and necessary prerequisites only. Run the specified acceptance checks and relevant regressions, including cross-scope retrieval, invalid lessons, and a previous incident whose cause differs from the current case.

Update docs/agent-handoff.md with implemented behavior, actual verification results, limitations, and Stage D readiness. Finish the bounded implementation; do not stop at a design proposal.







Implement Stage D of docs/copilot_agent_runtime_improvements_v1_0.md: observability, evaluation, feedback, and controlled improvement.

Read the shared constraints, Stage D requirements, docs/agent-handoff.md, and repository instructions. Inspect the existing tracing, evaluation, configuration, and release mechanisms.

After a brief gap assessment, implement:
- Sanitized traces with case/call identifiers, component versions, evidence references, usage, latency, errors, and stop reasons.
- Offline replay of recorded tool responses for application behavior checks.
- Structured reviewer feedback and candidate-change records.
- Repeatable baseline-versus-candidate evaluation using regression and held-out cases.
- Explicit reviewed release selection and selection of a previous release for rollback.

Reuse existing infrastructure. Preserve source-access restrictions, domain contracts, and earlier-stage behavior. Protect held-out reference answers from candidate development. Label mock evidence and unverified live behavior accurately.

The runtime agent must not modify its own production code, prompts, permissions, rules, tools, or approved memory. Manual candidate preparation and review are sufficient.

Run Stage D acceptance checks, demonstrate that a deliberately regressive candidate is detected, and run the existing end-to-end pilot demo and relevant regressions.

Update docs/agent-handoff.md with actual results and remaining gaps across Stages A–D. Distinguish completion of these runtime improvements from completion of the broader CyberArk quality workflow. Implement the bounded task rather than stopping at a plan.