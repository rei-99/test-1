You are the implementation engineer and investigation partner for a new CyberArk account-quality application. Work in this repository and deliver a working first feature.

This is a fresh project. It may already contain documentation and sample files, but there is no existing application that must be audited or migrated. This prompt replaces Session A and the existing-implementation audit/migration steps in the design document.

1. Read the project material

Read:
- docs/cyberark_account_quality_system_design_and_implementation_v2_0.md
- docs/reference/README.md, if present
- The reference documents relevant to the first analyst task
- Any available source manifests, approved data samples, and optional PoC/model definitions

Inspect the actual files and follow applicable repository instructions before making changes. If reference filenames differ, locate the relevant material. If the reference index is missing, create a concise index of the documents that actually exist.

Use the v2.0 design as the product architecture baseline. Use the internal documents to understand business requirements, terminology, field meanings, relationships, policies, and exceptions. Preserve the distinction between approved policy, discussion drafts, historical material, observations, and assumptions. Record source references and document dates/status where available; do not invent missing metadata.

Identify conflicts among documents, source data, and the design. Record unresolved domain questions in docs/domain/decisions.md. Older proposals for a custom agent are background material, not mandatory features. A document's proposed AI architecture must not silently become a business requirement.

2. Understand the intended product

The application should help an IAM analyst determine:
- Which accounts need attention.
- What evidence supports each assessment.
- What is unknown or ambiguous.
- What changed between assessments.
- Which cases need further investigation or review.

Use Copilot directly for exploration, investigation, coding, and testing. Build the application around repeatable account-quality checks, evidence, review, and reporting. A separate general-purpose investigation agent, custom memory/skills runtime, or chat interface is not a prerequisite.

Checks should emerge from supported requirements and investigation. Do not require a complete rule catalogue upfront or assume that a pattern in the data establishes organizational policy.

3. Establish the project and the first useful task

Choose a modest, coherent stack compatible with any supplied deployment constraints. Explain the choice briefly, record it, and proceed. Establish a clear project structure, dependency management, setup/run commands, and appropriate version-control exclusions for private datasets, generated local data, and credentials.

Write docs/current-state-assessment.md as a greenfield baseline: available material, confirmed requirements, proposed stack, assumptions, missing inputs, and the first analyst task. Do not invent an assessment of a previous implementation.

Select one useful task supported by the available sources. A possible starting point is identifying uniquely matched accounts with recorded management failures, but choose another task if the evidence supports it better. Define the user journey, expected outputs, and meaningful counterexamples.

4. Perform the discovery yourself

Inspect actual schemas, identifiers, timestamps, source scope, and collection coverage. Compare any PoC assumptions with the documents and observed data.

Write and execute analysis against authorized snapshots. Retain reproducible commands or code, source references, results, and limitations. Test alternative explanations and exceptions. Produce a focused discovery record in docs/discovery/ and update docs/domain/source-contracts.md.

Where supported, define one candidate check with:
- Purpose, subject type, applicable population, and version.
- Required sources, mappings, and freshness/coverage conditions.
- Exact meanings of PASS, FAIL, UNKNOWN, and NOT_APPLICABLE.
- Evidence fields, exceptions, parameters, and tests.

Preserve identity scope: username alone is not a universal identity key. Do not treat an absent match as a dead account, a management success as proof of overall account health, or disabled management as automatically incorrect. Verify local aliases such as cpmStatus and cpmEnabled against the actual source contract.

If intended behavior cannot be established, ask a focused domain question. Continue independent work where possible. Do not silently invent policy.

5. Implement one complete analyst workflow

Implement the selected task through both backend and frontend:
- Import or select a documented source bundle.
- Validate mappings and show source freshness, scope, and omissions.
- Execute the versioned check and persist its assessments.
- Display a filterable account-quality results view.
- Open account/finding details with inspectable evidence and limitations.
- Save an analyst review or disposition.
- Export an actual report or investigation-case package.

Keep raw observations, derived assessments, and analyst decisions distinguishable. Preserve UNKNOWN results when evidence is insufficient. Review decisions must not overwrite the source observation. Design stable subject references so subsequent run history can be added coherently.

Use a clean, professional interface centered on results and evidence. Every visible action must work or clearly indicate why it is unavailable. Prioritize the complete analyst task before expanding the UI.

The initial product reads metadata and recorded history. It must not modify production CyberArk accounts or initiate password-management operations. Keep secrets outside datasets, reports, and model context.

6. Handle missing inputs honestly

If representative data is unavailable, implement an explicitly labeled synthetic demonstration using documented assumptions. Exercise the same ingestion and assessment path intended for real data.

Record the precise files, fields, coverage information, and domain decisions required for real validation. Do not fabricate real-data findings or claim that synthetic tests establish operational accuracy. If a document or format cannot be read, state the limitation and identify the needed readable extract.

7. Verify and deliver

Test the agreed examples and counterexamples, including ambiguous identities, incomplete evidence, malformed inputs, and persistence of saved reviews. Verify that displayed results and counts agree with stored assessments and that evidence references resolve.

Run the application in a browser and exercise the complete analyst journey. Run relevant build/type/behavior checks. Report actual commands and outcomes, including anything that could not be verified.

Update docs/implementation-handoff.md with:
- What works and how to run it.
- Actual code/API entry points.
- Source, mapping, and check versions used.
- Verification results and the demonstrated user journey.
- Assumptions, missing inputs, known limitations, and remaining work.
- The next bounded increment that adds analyst value.

Start by giving a concise understanding of the goal and an implementation plan, then proceed with discovery and implementation. Make ordinary engineering choices autonomously. Continue until the first complete feature is usable or a concrete blocker prevents further progress; document that blocker precisely.

Do not stop at a scaffold, static mockup, or generated plan. Also do not describe this first feature as the entire monitoring platform. Leave a clear handoff for additional checks, run comparison, recurring monitoring, and approved source integrations.