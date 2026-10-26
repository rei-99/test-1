You are the implementation engineer for my Investigation Agent project.

Your task in this session is to establish the workspace and complete **R00 only**, following the supplied **v3.0 LangGraph design**. Make implementation changes and run the relevant checks.

The project is an early-stage local prototype. Its purpose is to investigate questions using code and database evidence, then produce a report with traceable findings and explicit uncertainty.

Follow these confirmed decisions:

- Use Python and LangGraph `StateGraph` as the sole agent orchestrator.
- Use `langchain-core` message/model interfaces and one suitable provider integration.
- Use SQLite for the investigated database.
- Use a separate SQLite file for LangGraph checkpoints.
- Keep the DB MCP server as an independent project/process using the official MCP Python SDK over stdio.
- Preserve application contract version `2.1`; the architecture version is `3.0`.
- Keep production security auditing, enterprise authentication, UI, hosting, multi-agent delegation, and automatic checkpoint recovery outside the initial scope.

**First, inspect and prepare the workspace.**

1. Locate the expanded design directory, normally named `investigation-agent-design`.
2. If only `bootstrap_investigation.py` exists, run it to create a new design directory, then run its `--verify` command. Never expand over an existing directory.
3. If the design directory already exists, use it directly. Do not load the entire embedded bootstrap payload into your context.
4. Locate existing implementation projects. Otherwise, use sibling directories named `db-mcp-server` and `investigation-agent`, creating only the scaffolding needed for R00.
5. Preserve existing work and suitable project conventions. Record any mapping between actual paths and the proposed layout.

My company environment allows viewing and copying GitHub content but does not allow downloading or cloning this delivery pack. Use the locally supplied bootstrap and generated files. Do not ask me to transfer fixtures, examples, or task cards individually.

**Read the implementation guidance selectively.**

Start with these files inside the design directory:

- `README.md`
- `IMPLEMENTATION_WORKFLOW.md`
- `tasks/R00.md`
- Relevant sections of `investigation_agent_architecture_design.md`
- Relevant sections of `CONTRACTS.md`

The v3.0 design supersedes earlier orchestration choices. Follow the detailed R00 task card as the acceptance authority. Load other files only when needed.

**Complete R00 with executable evidence.**

- Inspect the available Python, Git, SQLite, and dependency-management environment.
- Resolve and pin compatible dependency versions using actual available packages and public APIs.
- Build a minimal LangGraph spike covering state updates, `add_messages`, conditional routing, runtime context, and correctly paired `AIMessage`/`ToolMessage` call IDs.
- Verify SQLite checkpoint persistence by closing and reopening a real checkpoint file and reading saved state without executing investigation nodes.
- Verify a real local stdio MCP exchange covering structured success, tool failure, and subprocess cleanup.
- Use deterministic scripted model responses. R00 must not require model credentials or paid model calls.
- Record the selected provider interface and distinguish locally verified behavior from live-endpoint compatibility that remains untested.

Create only the minimal code, tests, dependency files, and documentation needed for these checks. The full Agent, DB tool implementation, and later task cards remain subsequent work.

Make routine implementation decisions autonomously within the design. If dependency installation or another environment limitation blocks a check, complete independent work and record the exact blocker. Do not invent APIs, claim skipped checks passed, or silently weaken acceptance criteria.

Record dependency/API decisions in `docs/decisions/runtime-baseline.md` and progress in `docs/progress.md`, noting their actual workspace locations.

When finished, report:

1. Workspace layout and files created or changed.
2. Exact dependency versions selected.
3. Commands executed and their actual results.
4. Each R00 acceptance criterion marked PASS, FAIL, or NOT RUN.
5. Remaining blockers or necessary design adjustments.
6. The next ready task and a short handoff.

Stop after the R00 handoff. Start now by inspecting the workspace and executing this task.
