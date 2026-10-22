Set up a simple VS Code + GitHub Copilot workspace configuration for the current project. Create the files directly.

Context:

* The current workspace root represents one larger project.
* It contains multiple components, each cloned from a separate GitLab repository.
* Each component keeps its own Git history, branches, and commits.
* I want Copilot to understand the overall project, locate the relevant components, and handle changes across components when necessary.
* Keep the setup simple. Create or update only the two workspace-level files described below.

First, inspect the component directories in read-only mode. Review their README files, build files, existing AI instructions, and only the code or configuration needed to understand their relationships.

1. Create .github/copilot-instructions.md

Write concise, actionable instructions covering these points:

* This workspace contains multiple independent Git repositories.
* At the beginning of a task, read the root-level PROJECT.md, identify the relevant components, and inspect their code as needed.
* Before modifying a component, read and follow its existing applicable AI instructions and development conventions.
* Run Git, build, startup, and test commands from the appropriate component directory.
* When changing interfaces, data structures, or shared dependencies, check related callers and affected components.
* Keep changes focused and follow each component’s existing coding style.
* At completion, summarize changes and validation results by component, including anything that remains unverified.
* Explain work in Chinese by default, while following existing project conventions for code and identifiers.

Reference PROJECT.md using the correct relative Markdown link: [Project overview](../PROJECT.md). Keep detailed project information in PROJECT.md to avoid duplication.

2. Create PROJECT.md

Based on evidence from the workspace, briefly document:

* The overall purpose of the project.
* A component table containing each component’s name, relative path, and main responsibility.
* Important calls and dependencies between components.
* Common build, startup, and test commands for each component.
* Key code entry points and essential development notes.

Requirements:

* Base factual statements on existing code, configuration, or documentation.
* Label inferred relationships as “Needs confirmation” and use TODO for unknown information.
* Verify commands against existing README files, scripts, or build configurations. Do not invent commands.
* Keep both files concise and link to existing documentation for details.
* Preserve and integrate existing content if either file already exists.
* Keep component source code, existing component instructions, and Git configuration unchanged during this setup.

After creating the files, briefly report:

* Which files were created or updated.
* Which components and relationships you identified.
* Which details I still need to provide or confirm.
