# Copilot implementation prompt: autonomous investigation frontend

**Version 1.0 · 21 September 2026**

Companion to `autonomous_cyberark_discovery_agent_design_v1_0.md`.

This entire document is an implementation brief for Copilot. Place it under `docs/` in the new autonomous-agent project. The short launch prompt at the end is ready to paste into a fresh Copilot session.

## 1. Role and product objective

Act as a senior frontend engineer and product designer. Build a polished, functional web workspace for the autonomous CyberArk discovery agent described in the companion design.

The experience should feel like a modern agent application: a persistent conversation, visible work in progress, useful context controls, and real outputs that can be opened beside the conversation. Use Codex-style interaction patterns as inspiration while giving this application its own identity. Suggested working name: **Identity Lab**. Do not imply affiliation with OpenAI or CyberArk.

The user starts with a mission and source material. The agent explores, investigates, asks questions, and produces evidence-backed findings, candidate checks, code, tests, and reports. Existing findings or a quality rule engine are not prerequisites. Make this discovery workflow the center of the interface.

The result must be a working application with meaningful interactions, not just a static mockup. Implement a frontend against the actual backend where available and a clearly identified demo adapter where necessary. Report integration limits accurately.

## 2. Repository inspection and scope

Read the companion design, applicable repository instructions, and `docs/agent-handoff.md` if present. Inspect the new project's backend, API definitions, run lifecycle, events, artifact storage, authentication, existing frontend, and tests before selecting interfaces.

Use the current frontend stack if one is established. Otherwise, default to React, TypeScript, Vite, and a small accessible component foundation with CSS variables or Tailwind for styling. Follow current official documentation when installing packages; retain a lockfile and avoid unnecessary dependencies. Use the existing backend language and framework for any small integration bridge.

Keep this task focused on the frontend and minimal API adaptation. Do not replace the autonomous agent with a second implementation. If the backend is CLI-only or incomplete, document the required bridge and keep demo transport behind the same typed client interface used by live transport. Build the usable demo while identifying missing live contracts.

Before editing, give a brief implementation plan and note material backend gaps. Then implement the planned UI. Preserve unrelated work and resolve routine design choices independently.

## 3. Visual direction

Build a calm, precise workspace with strong typography and restrained color. Prioritize readable conversations, clear hierarchy, and quick inspection of evidence.

Use these starting design choices, adapting to an existing coherent design system:

| Element | Direction |
|---|---|
| Light theme | Warm near-white canvas, white content surfaces, subtle gray boundaries |
| Dark theme | Charcoal canvas and slightly lighter panels; readable text and quiet borders |
| Accent | One restrained indigo accent for primary actions, selection, and focus |
| Typography | System sans-serif or an available Inter font; monospace for code and identifiers |
| Density | Approximately 14 px interface text and 15–16 px conversation text, with comfortable line spacing |
| Spacing | Consistent 4/8 px spacing rhythm; generous space around messages and compact utility controls |
| Shape | Modest corner radii, thin separators, and subtle elevation for overlays |
| Motion | Short transitions for panels and activity; respect reduced-motion preferences |

Use simple, consistent icons with accessible labels. Keep success, attention, and error colors semantic. Empty states should explain the next useful action. Avoid oversized KPI cards, decorative charts, neon effects, and a landing-page hero inside the workspace.

Provide light, dark, and system theme options. Show long identifiers, filenames, tables, and code without breaking the layout. Verify both themes in a real browser rather than relying only on compilation.

## 4. Workspace layout

Use three cooperating regions on a wide desktop:

| Region | Content and behavior |
|---|---|
| Left sidebar, around 250 px | Workspace selector if supported, New mission, mission search/history, active-run indicators, Sources, Skills, and Settings |
| Main area, flexible | Mission header, conversation, inline activity groups, operator questions, and a persistent composer |
| Right inspector, around 420 px | Evidence, artifacts, and context opened from conversation references; collapsible, resizable, and optionally expandable |

Keep the conversation readable at roughly 700–850 px maximum text width when space permits. The sidebar and inspector must yield space at smaller widths rather than compressing the main view into an unusable column.

Suggested responsive behavior: three panes at approximately 1280 px and above; a collapsible navigation rail and overlay inspector on medium screens; one primary view with drawers or separate detail views below approximately 900 px. At phone widths, preserve the composer, navigation, artifact access, and stop control without horizontal page overflow.

Persist theme, panel widths, and other harmless layout preferences. Use routes for mission selection and inspector references so refresh, back navigation, and direct links behave predictably. Do not store source contents or credentials in preference storage.

## 5. Core user journeys

### Start a mission

The new-mission screen should contain a focused composer, a source/context picker, and three or four example missions. Examples populate an editable draft; they do not launch automatically.

Suggested examples:

- Understand these account datasets and identify promising quality investigations.
- Investigate management reliability and propose checks supported by the history.
- Explore relationships between safes and business applications.
- Examine possible identity inconsistencies and explain ambiguous matches.

Let the user attach or select authorized datasets, documentation, and optional PoC source references. Present selected context as removable chips with an inspect action. Show scope and advanced resource options compactly; derive valid values from the backend.

Support file selection and drag/drop for formats advertised by the ingestion service. Distinguish upload accepted, processing, ready, and failed. Show size/type errors and retry actions. Do not show an unsupported file as successfully ingested. A built-in synthetic source bundle should make the demo usable without credentials.

Allow an exploratory mission without precomputed findings. When useful evidence is missing, show the agent's focused request for it. Do not force every source category to be present before a mission can start.

### Follow an investigation

Render user messages, assistant updates, completed responses, tool activity, questions, evidence references, and artifact cards as distinct item types in one coherent conversation.

Stream assistant text where supported. Group repetitive tool activity into compact expandable rows, such as “Inspecting three source schemas” or “Testing an ownership hypothesis.” Expanding a row can show its actual inputs, bounded result, duration, and evidence references. Use backend-provided activity and sanitized summaries; do not fabricate progress messages or expose hidden chain-of-thought.

Show the current objective and a small adaptive task list if the backend supplies one. The list can change as the investigation develops. Display superseded or blocked steps honestly instead of presenting a fixed business pipeline as the agent's plan.

Keep automatic scrolling only while the user is near the latest message. If they scroll upward, preserve their position and show a “Jump to latest” action. Render long histories and tables efficiently without breaking keyboard access or message anchors.

### Steer, answer, stop, and continue

The composer supports multiline input, attachments, and contextual follow-ups. Sending during a run must follow backend capabilities: immediate steering, queued instructions, or unavailable. Show the acknowledged state clearly. A queued message is not already applied to the investigation.

When the agent asks a domain question, render a focused answer card with any backend-supplied choices, a free-text option, and an “I don't know” response. Submit against a stable question identifier; preserve text on failure and handle questions that have already been resolved.

Provide a visible Stop action for an active attempt. Show “Stopping” while cancellation is pending and the confirmed state after acknowledgment. Closing a browser tab or changing missions must not silently cancel server work. Resume or Continue appears only when supported by the backend's actual attempt semantics; retain the existing mission budget and history as reported by the server.

Prevent duplicate sends and starts through pending states and idempotent request identifiers. Preserve an unsent draft during navigation within the app; choose session-only handling unless persistent draft storage is approved for the data involved.

### Inspect evidence and hypotheses

Clicking a citation opens the corresponding evidence in the inspector without losing conversation position. Show a readable source name, snapshot/time, relevant rows or passage, query/analysis reference, coverage limitations, and a link back to the message.

Distinguish observations, hypotheses, candidate checks, and unknowns using concise text labels. Where supported, show both supporting and conflicting evidence. Do not invent confidence percentages or treat “no result returned” as a proven absence.

For a hypothesis, show its concise statement, status, evidence references, and the next unresolved question when available. Feedback such as “This is an intentional exception” should attach to the specific item and version, with the user's explanation. Feedback submission is not automatic policy approval.

### Review and use outputs

Present generated reports, source notes, SQL/Python analyses, candidate check definitions, tests, and workflow proposals as versioned artifacts. Each card should open a real artifact, show its type and status, and offer supported copy/download actions.

The inspector supports safe Markdown preview, code with syntax highlighting, JSON, and bounded table previews for supported formats. Provide a raw/source view where useful. Unsupported formats get metadata and a download action instead of a broken preview.

When two text versions exist, provide a readable diff and version selector. Keep the selected immutable version visible; do not replace it silently when a newer version arrives. Display validation outcomes as reported: generated, tested on a named fixture, failed, or not evaluated. Generated code is not automatically verified or deployed.

Support exporting the actual monitoring package when the backend exposes it. Use accurate action labels such as “Download package,” “Request revision,” and “Submit feedback.” The frontend must not introduce account cleanup or production execution controls absent from the backend design.

## 6. Sources, skills, and context visibility

Provide a compact Sources view showing available datasets/documents, ingestion state, relevant scope, observation times, and limitations. Distinguish sources available in the workspace from those included in the current mission. Adding a source during a run follows backend rules and must not silently change pinned evidence.

Provide a Skills browser with names, descriptions, versions, applicability, and supported activation controls. “Auto” lets the runtime choose skills; explicit selection is a request validated by the backend. Distinguish available, requested, and active skills.

The Context inspector shows selected sources, the active runtime instruction version, active skills, and relevant memory references when exposed by the backend. Keep raw prompt diagnostics in an optional developer detail view. Explain context in terms useful to an analyst; do not crowd the normal workflow with protocol fields or token accounting.

Treat runtime `AGENTS.md`, skills, and reviewed memory as backend-managed assets. The browser can display authorized content and submit supported requests. It must not read arbitrary workspace paths or silently edit deployed instructions.

## 7. Backend client and event handling

Create a typed `AgentClient` or equivalent adapter and keep transport details outside visual components. Match actual backend contracts first. If contracts are absent, document the proposed minimal shape and implement the demo adapter against it.

The client should cover the needed operations: list/read missions; create a mission and start an attempt; send messages; answer questions; request cancellation; read canonical state/history; subscribe to events; inspect sources/evidence/skills; upload supported files; and fetch versioned artifacts. Make capabilities explicit so unavailable actions can be omitted or clearly disabled.

Use the server's existing streaming mechanism. If choosing a new one, HTTP mutations plus an authenticated event stream is a sufficient starting point; bidirectional WebSockets are not a requirement. Keep provider API keys and agent execution on the backend. The UI need not depend on Codex's runtime or protocol to provide a similar experience.

Proposed event envelope, to adapt to existing APIs:

```typescript
type AgentEvent = {
  eventId: string;
  missionId: string;
  attemptId: string;
  sequence: number;
  occurredAt: string;
  schemaVersion: number;
  type: string;
  payload: unknown; // Validate into a discriminated union at the boundary.
};
```

Events may cover message updates, activity starts/completions, plan or hypothesis changes, questions, evidence, artifact versions, skills, usage, and attempt status. Keep conversation history and canonical mission state separate from transient streaming display.

Implement the following behavior:

- Deduplicate by event identity and reconcile ordering with the backend's cursor/sequence semantics. Detect gaps and fetch a canonical snapshot or missing events.
- Reconnect with bounded backoff and the last acknowledged cursor where supported. Show connection state separately from agent state: a dropped stream does not mean the agent failed or stopped.
- Cancel stale frontend subscriptions when changing missions, without cancelling the agent. Late events must not appear in the newly selected mission.
- Reconcile partial streamed messages with final persisted content; do not append the final text twice. Do not mark an attempt complete merely because the connection closed.
- Refetch after ambiguous mutation outcomes and use idempotency keys. A retry must not start a duplicate investigation.
- Restore history and current status after refresh. Treat the backend as authoritative for state transitions, including a late completion during cancellation.

Show explicit queued, running, waiting-for-input, stopping, completed, incomplete, cancelled, and failed states where the backend supports them. Map backend terminology deliberately. Use activity indicators and elapsed time rather than invented percentage completion. Show cost or token usage only when supplied, labeling estimates and unavailable values.

## 8. Demo mode and honest integration

Provide an explicit Demo mode using synthetic data and deterministic event fixtures. It must exercise the same UI client contract as live mode and remain visibly labeled. Never silently substitute simulated success when a live request fails.

Include one coherent scenario that demonstrates:

1. Starting a mission with source context and no existing findings.
2. Source inspection, a hypothesis, and an analysis activity.
3. A focused domain question and the user's answer.
4. A result with both supporting evidence and a limitation.
5. A generated check/report, its evaluation result, and a later artifact revision.
6. Interruption or connection loss followed by correct state restoration.

Use separate short fixtures for missing data, rejected uploads, invalid evidence references, unavailable skills, cancellation races, and a failed attempt. Do not preload the real user's interface with fabricated account-quality statistics.

## 9. Accessibility, content handling, and interaction quality

Use semantic controls, visible focus states, sufficient contrast, and accessible names for icons. Menus, dialogs, tabs, resizing controls, and source pickers must work by keyboard. Manage focus when opening and closing panels. Announce important status changes without reading every streamed token through a live region.

Support a command palette with the normal Ctrl/Cmd+K convention, unless the existing app reserves it. Include actions such as New mission, Find mission, Open sources, Open artifacts, and Switch theme. Choose and document a composer send shortcut; preserve multiline editing and never submit during IME composition.

Treat model output, source contents, filenames, and artifacts as untrusted content. Render Markdown and code without executable HTML, restrict unsafe link schemes, and avoid executing generated code in the browser. Use backend-issued artifact references rather than arbitrary filesystem paths. Preserve authorization errors without leaking protected content.

Every visible action must work or clearly explain its unavailable state. Destructive actions, unsupported provider switches, synthetic metrics, and production deployment controls must not appear as decorative buttons.

## 10. Implementation sequence

Build incrementally inside this frontend task:

1. **Working workspace:** establish design tokens, routing, responsive layout, mission history, composer, and a typed client with the initial demo fixture.
2. **Agent interaction:** implement streaming activity, questions, steering/cancellation semantics, navigation, refresh, and reconnection behavior.
3. **Inspectable outputs:** implement citations, evidence and artifact panes, previews, versions/diffs, source context, and skills visibility.
4. **Integration and polish:** connect available live endpoints, finish error/empty states and accessibility, verify responsive layouts, and document remaining backend gaps.

Do not stop after the first attractive screen. Keep advanced infrastructure optional, but complete the core demo journey and every supported live interaction. If a real backend capability is missing, finish the demonstrable UI path through the explicit adapter and identify the exact integration work left.

## 11. Verification and handoff

Use the existing test stack, or add focused browser tests if none exists. Verify behavior rather than implementation details. Cover the main mission journey, citation navigation, artifact download/version switching, question submission, cancellation, refresh/reconnect, duplicate events, and mission switching during a live stream. Include a malicious Markdown fixture and a keyboard-only pass through the primary flow.

Run the app in a browser. Inspect at least a wide desktop view around 1440×900, a medium view around 1024×768, and a phone view around 390×844. Capture screenshots of the empty workspace, an active investigation, and an artifact/evidence inspector. Check long content, scroll behavior, composer visibility, overlays, and both themes. Fix concrete visual issues before declaring the UI complete.

Run relevant type, build, and behavioral checks. Report commands and observed results, including unavailable tooling or backend dependencies. Mock tests establish frontend behavior, not autonomous model quality or production integration.

Deliver:

- Frontend source with a coherent component structure and typed backend adapter.
- Explicit demo mode and documented live integration configuration.
- Working core interactions, supported previews, and meaningful tests.
- Screenshots from the actual running UI and concise setup/run instructions.
- A backend integration note describing capabilities, event/state mapping, and unresolved gaps.
- An updated `docs/agent-handoff.md` with actual implementation references, results, and next tasks.

## 12. Reference and interpretation

The [official OpenAI desktop workspace guide](https://learn.chatgpt.com/docs/app) describes organizing ongoing work and inspecting outputs within one workspace. Those broad interaction ideas inform this brief. The three-pane layout, domain views, event contract, design tokens, and acceptance criteria above are proposed choices for Identity Lab; they are not claims about Codex internals or a requirement to reproduce its branding.

## Copy-and-paste launch prompt

```text
Read docs/frontend_prompt.md and docs/autonomous_cyberark_discovery_agent_design_v1_0.md. Implement the frontend described in the frontend brief.

Build a polished Codex-inspired agent workspace centered on missions, conversation, live investigation activity, evidence, and generated artifacts. The user starts with a goal and source material; quality checks are outputs, so do not require existing findings or build a rule-first dashboard.

Inspect the new project's actual backend, contracts, repository instructions, and handoff. Reuse an established frontend stack, or apply the brief's defaults. Give a short plan, then implement through the working workspace, agent interaction, output inspection, and integration/polish stages.

Use a typed backend client and explicit synthetic Demo mode where live capabilities are missing. Implement honest state, streaming/reconnect, question/steering/cancellation behavior, evidence citations, artifact previews and versions, and context/skills visibility. Keep agent execution and credentials on the backend.

Verify the core journey in a real browser, inspect desktop and mobile layouts in both themes, fix visual issues, and run relevant behavioral/build checks. Update docs/agent-handoff.md with actual code references, results, screenshots, limitations, and next steps. Deliver a working interface rather than a static mockup or a plan alone.
```
