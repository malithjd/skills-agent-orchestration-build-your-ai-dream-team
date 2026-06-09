# Copilot instructions for this repository

Purpose
- This repository is an interactive exercise (Project Pulse dashboard). The `.github/agents/` files encode the expected behavior for specialized Copilot agents (Orchestrator, Planner, Coder, Designer). Use those files as authoritative instructions for agent roles.

Build, test, and lint commands
- No build/test/lint toolchain configured in this repo.
- Preview the static dashboard locally:
  - Serve the app directory, e.g.:
    - `python -m http.server 8000 --directory app` or
    - `npx http-server app` (if node is available)
  - Open http://localhost:8000/index.html
- Tests: none present. If tests are added, run them using the test runner declared in the repo (e.g., `npm test`, `pytest`).

High-level architecture (big picture)
- Purpose: a small static frontend (app/) that shows a Project Pulse dashboard (project cards, status badges, recent activity).
- Core files expected by the exercise:
  - `app/index.html` — main dashboard HTML
  - `app/styles.css` — styling for dashboard
  - `app/project-data.json` — top-level `projects` array used by the frontend
- Orchestration model: the repository is organized as a multi-agent exercise. The Orchestrator breaks work into file-scoped phases and delegates to Planner, Coder, and Designer agents. See `.github/agents/*.agent.md` for each agent's responsibilities and rules.
- Workflows: `.github/workflows/` contains exercise orchestration workflows (start/step enabling). They manage the learning flow, not CI for builds/tests.

Key repository-specific conventions
- Agent rules (must-follow): the agent files declare explicit "Git control" rules — agents must NOT stage, commit, or push changes. The human learner controls git operations via Copilot CLI prompts.
- File-scope delegation: break work into explicit file ownership to avoid overlapping edits. Run parallel tasks only when file scopes do not overlap.
- Runnable app support: when assigned, Coder must create `.vscode/launch.json` with `cwd: ${workspaceFolder}/app` and open `index.html` (strict JSON, no comments).
- Frontend data shape: `app/project-data.json` must contain a top-level `projects` array; each project object should include `name`, `owner`, `status`, `recentActivity`, and `priority`.
- CSS hooks: Designer expects deterministic CSS selectors like `.dashboard` and `.project-card` for styling and testability.
- Validation expectations: Coder should validate changes locally (open the launch configuration or load index.html) and report what was validated and any risks.

Where to find guidance and next steps
- Agent role definitions: `.github/agents/orchestrator.agent.md`, `.github/agents/planner.agent.md`, `.github/agents/coder.agent.md`, `.github/agents/designer.agent.md`.
- Exercise step content: `.github/steps/*.md` and `.github/workflows/*` — these orchestrate the learner-facing exercise flow.

Notes for future Copilot sessions
- Read the agent files before acting. They contain rules the exercise expects agents to follow.
- If asked to implement the dashboard, prefer creating the three app files above and the `.vscode/launch.json` support file.
- Don't assume test/build infrastructure exists; verify presence of `package.json`, `pyproject.toml`, or similar before invoking language-specific toolchains.

If you'd like, I can add an optional GitHub Actions job or MCP server configuration to serve and/or test the static app (Playwright/visual regression). Would you like that?  
