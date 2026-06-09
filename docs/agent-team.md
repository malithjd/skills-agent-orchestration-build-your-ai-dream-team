# Agent team

Below is the custom agent team for building Mona's Project Pulse dashboard.

- Orchestrator — model: Claude Opus 4.7 (copilot)
  - Responsibility: coordinate Planner, Coder, and Designer; break work into phases, assign file scopes, and verify integrated results.
  - Definition: .github/agents/orchestrator.agent.md

- Planner — model: Claude Opus 4.7 (copilot)
  - Responsibility: research the repo, produce ordered implementation steps, file assignments, dependencies, and validation guidance.
  - Definition: .github/agents/planner.agent.md

- Coder — model: GPT-5.5 (copilot)
  - Responsibility: implement code, create runnable app support (e.g., .vscode/launch.json), validate changes, and report risks.
  - Definition: .github/agents/coder.agent.md

- Designer — model: Gemini 3.1 Pro (copilot)
  - Responsibility: design UI/UX for the dashboard—project cards, status badges, responsive layout, and accessibility guidance.
  - Definition: .github/agents/designer.agent.md

This work is orchestrated using the GitHub Copilot CLI running in a Codespace.