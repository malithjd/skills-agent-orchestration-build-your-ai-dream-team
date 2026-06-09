Assumptions
- The repository is a plain static site; no build toolchain (webpack, Node scripts) will be assumed.
- The site will be previewed using a simple static server (python -m http.server or npx http-server) from the app/ folder.
- The frontend will fetch app/project-data.json at runtime (so preview must use an HTTP server, not file://).
- Keep everything minimal and deterministic for automated tests: explicit CSS selectors (.dashboard, .project-card), strict JSON shape, and a .vscode/launch.json with cwd set to ${workspaceFolder}/app.
- The plan is conservative and aimed at a single small deliverable: a static Project Pulse dashboard frontend.

Summary
This plan defines a minimal static frontend implementation for a “Project Pulse” dashboard. Deliverables include four exact files:
- app/index.html
- app/styles.css
- app/project-data.json
- .vscode/launch.json

High-level objectives:
- Provide a static HTML page that loads a JSON file of projects and renders them as project cards inside a container with selector .dashboard.
- Provide deterministic CSS selectors for tests (.dashboard and .project-card).
- Ensure a sample project-data.json exists with the required shape.
- Add a .vscode/launch.json that sets cwd to ${workspaceFolder}/app to help local preview/debug in VS Code.

High-level implementation plan (phases + rough effort)
1. Phase 0 — Kickoff & discovery (0.5–1 hour)
   - Confirm requirements, file list, JSON shape, and preview method.
2. Phase 1 — Data & scaffolding (1–2 hours)
   - Create app/project-data.json (sample entries).
   - Create app/index.html minimal shell that fetches the JSON and renders project cards.
3. Phase 2 — Design & stylesheet (1–2 hours)
   - Create app/styles.css with deterministic selectors (.dashboard, .project-card) and minimal responsive rules.
4. Phase 3 — VS Code preview config (0.25 hour)
   - Create .vscode/launch.json with cwd set to ${workspaceFolder}/app.
5. Phase 4 — Validation & QA (0.5–1 hour)
   - Manual validation, run local server, check data shape and CSS selectors, cross-browser spot-check.
6. Phase 5 — Documentation & handoff (0.25–0.5 hour)
   - Save this plan as docs/project-pulse-plan.md and check-in instructions for the Orchestrator.

Estimated total: ~3.5–7 hours (single person). With parallel work (designer + coder), wall-clock time can be less.

File-scoped task breakdown (Designer vs Coder)
General guidance: "Designer" focuses on look, copy, and CSS rules; "Coder" focuses on file structure, JSON shape, DOM building, and validation.

1) app/project-data.json (Coder primary, Designer consult)
- Tasks:
  - Create app/project-data.json with top-level "projects" array (create).
  - Populate with 3 example project entries (create).
  - Validate JSON syntax and required fields: each project object must include name, owner, status, recentActivity, priority (validate).
- Deliverable: app/project-data.json (strict JSON).
- Acceptance: JSON parses, contains "projects" array, each entry has the 5 required fields.

2) app/index.html (Coder primary, Designer advises content/layout)
- Tasks:
  - Create a minimal static HTML file that:
    - Links app/styles.css.
    - Contains a main container element with class="dashboard".
    - Loads app/project-data.json using fetch (or embedded fallback) and renders project cards.
    - Uses elements consistent with the CSS hooks (each card must have class="project-card").
    - Provide minimal accessible markup (heading for page, aria labels as appropriate).
  - Validate that for each project object rendered, the DOM contains an element with .project-card and child elements showing name/owner/status/recentActivity/priority.
- Deliverable: app/index.html (static, no server-side).
- Acceptance: Page renders via HTTP server, project entries appear as project cards.

3) app/styles.css (Designer primary, Coder validates)
- Tasks:
  - Create app/styles.css (create).
  - Provide deterministic selectors required by tests:
    - .dashboard (root container).
    - .project-card (card for each project).
  - Implement minimal responsive styling and visual priority/status cues (colors or badges).
  - Keep selectors simple and deterministic (avoid generated class names and complex combinators).
- Deliverable: app/styles.css.
- Acceptance: CSS file loads, .dashboard styles the container, .project-card styles each card. Visuals consistent and contrast accessible.

4) .vscode/launch.json (Coder)
- Tasks:
  - Create .vscode/launch.json (create).
  - It must be strict JSON with a configuration that sets cwd to "${workspaceFolder}/app".
  - Validate the JSON file parses and that cwd is exactly the required value.
- Deliverable: .vscode/launch.json.
- Acceptance: JSON is valid and contains cwd: "${workspaceFolder}/app" in at least one configuration.

Dependencies (dev/runtime) and preview commands
- No external dev dependencies required by default.
- Runtime: modern browser.
- Preview commands (run from repository root or adjust cwd as shown):
  - Using Python 3: cd app && python3 -m http.server 8000
  - Using Node http-server (if Node installed): cd app && npx http-server -p 8000
- If using fetch to load project-data.json, serving over HTTP is required (file:// will block fetch).
- Optional future dev dependencies (not required now): ajv (JSON Schema validator), lint tools.

Parallelization decisions
Work that can run in parallel:
- Designer can start app/styles.css in parallel with Coder creating app/project-data.json and app/index.html scaffold (styles can be developed concurrently).
- While JSON is being finalized, Designer can prepare visual assets (icons) and color tokens used in CSS.
Reasons: CSS is independent of data content and skeleton HTML; index.html can render placeholder content which CSS will style.

Work that must run sequentially:
- The final integration and QA must be sequential: index.html → pull project-data.json → render → style check.
- .vscode/launch.json creation can occur at any time but should be validated once app/ exists; no hard dependency.

Implementation steps (ordered) with exact file assignments and minimal detail
Phase 1 — Scaffolding (Coder)
1. Create app/project-data.json
   - File: app/project-data.json
   - Create JSON with top-level "projects" array; include 3 examples.
   - Validate JSON with a parser or jq.

2. Create app/index.html (initial scaffolding)
   - File: app/index.html
   - Minimal HTML structure:
     - <head> linking styles.css
     - <main class="dashboard" id="dashboard"></main>
     - A <script> at bottom that fetches project-data.json and populates #dashboard with .project-card elements.
   - Ensure DOM includes .dashboard and that each card gets class .project-card.

Phase 2 — Styling (Designer)
3. Create app/styles.css
   - File: app/styles.css
   - Provide styles for .dashboard, .project-card, and small responsive rules.
   - Ensure deterministic selectors (.dashboard and .project-card exist and are usable for tests).

Phase 3 — VS Code config (Coder)
4. Create .vscode/launch.json
   - File: .vscode/launch.json
   - Provide at least one configuration object with cwd set to "${workspaceFolder}/app".
   - Strict JSON (no comments).

Phase 4 — QA & Validation (Coder + Designer)
5. Validate the integration by running a local server and checking the page renders and styles apply correctly.
   - Run: cd app && python3 -m http.server 8000
   - Open http://localhost:8000/index.html
   - Verify the JSON loads, DOM renders .project-card elements inside .dashboard, CSS applied, and the .vscode launch file exists and parses.

Phase 5 — Documentation (Coder)
6. Save this plan to docs/project-pulse-plan.md (create/modify).
   - File: docs/project-pulse-plan.md
   - Include plan contents, sample snippets, and validation steps.

Parallel vs sequential mapping summary
- Parallel: styles.css work (Designer) can run in parallel with project-data.json creation and index.html scaffold (Coder).
- Sequential: final integration, fetch-based rendering, and QA must be sequential after all three core files exist.

Example JSON schema and sample data
- Required shape:
  - Top-level object with key "projects" → array.
  - Each project object must include:
    - name (string)
    - owner (string)
    - status (string) — e.g., "on track", "at risk", "blocked"
    - recentActivity (string, ISO 8601 recommended or short text like "2 days ago")
    - priority (string or integer; choose a small consistent set — e.g., "low", "medium", "high")
- Example app/project-data.json content (strict JSON):

{
  "projects": [
    {
      "name": "Project Atlas",
      "owner": "Alice Nguyen",
      "status": "on track",
      "recentActivity": "2026-06-08T12:34:00Z",
      "priority": "high"
    },
    {
      "name": "Bridge Revamp",
      "owner": "Carlos Mendez",
      "status": "at risk",
      "recentActivity": "2026-06-07T09:15:00Z",
      "priority": "medium"
    },
    {
      "name": "Integration Sprint",
      "owner": "Dana K.",
      "status": "blocked",
      "recentActivity": "2026-06-05T16:00:00Z",
      "priority": "high"
    }
  ]
}

(Place the above JSON exactly in app/project-data.json.)

Example .vscode/launch.json (strict JSON)
- Minimal example that sets cwd to ${workspaceFolder}/app:

{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Preview Project Pulse (Chrome)",
      "type": "pwa-chrome",
      "request": "launch",
      "cwd": "${workspaceFolder}/app",
      "url": "http://localhost:8000/index.html",
      "webRoot": "${workspaceFolder}/app"
    }
  ]
}

Sample HTML structure (snippet for index.html)
- Provide minimal snippet showing .dashboard and .project-card markup. The final index.html should include similar structure and a small script to fetch project-data.json and render the cards.

<body>
  <header>
    <h1>Project Pulse</h1>
  </header>

  <main class="dashboard" id="dashboard" aria-live="polite">
    <!-- JavaScript will inject .project-card elements here -->
    <!-- Example of a single card -->
    <article class="project-card" role="region" aria-label="Project Atlas">
      <h2 class="project-name">Project Atlas</h2>
      <div class="project-meta">
        <span class="project-owner">Alice Nguyen</span>
        <span class="project-status">on track</span>
        <span class="project-activity">2026-06-08T12:34:00Z</span>
        <span class="project-priority">high</span>
      </div>
    </article>
  </main>

  <script>
    // Minimal client-side fetch and render (the actual implementation lives in index.html)
    // Fetch app/project-data.json, iterate projects, create elements with class="project-card"
  </script>
</body>

CSS hooks designers should implement (sample)
- File: app/styles.css must define at least these selectors (examples to guide design):
  - .dashboard { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1rem; padding: 1rem; }
  - .project-card { background: #fff; border: 1px solid #ddd; padding: 0.75rem; border-radius: 6px; box-shadow: 0 1px 0 rgba(0,0,0,0.03); }
  - .project-name, .project-owner, .project-status, .project-activity, .project-priority — optional sub-selectors for styling.
- Keep selectors simple and deterministic; tests will look for .dashboard and .project-card specifically.

Validation and acceptance criteria
What to check after implementation:

1) Data shape validation
- Manual check:
  - Open app/project-data.json and confirm top-level "projects" array exists.
  - Each entry must include keys: name, owner, status, recentActivity, priority.
- Quick jq checks (command-line):
  - Ensure top-level:
    - jq 'has("projects")' app/project-data.json
  - Ensure there is at least one project:
    - jq '.projects | length' app/project-data.json
  - Check required fields presence for every project:
    - jq -e '.projects[] | [ .name, .owner, .status, .recentActivity, .priority ] | map(type == "string" or type == "number") | all' app/project-data.json
  - (If jq not available, use any JSON validator or open in editor.)

2) CSS selectors validation
- Manual DOM inspection:
  - Start server: cd app && python3 -m http.server 8000
  - Open http://localhost:8000/index.html
  - In devtools Elements panel, confirm:
    - There is a container element with class "dashboard".
    - For each project listed in JSON, there is an element with class "project-card".
- Automated test idea (later): small integration script using Puppeteer or Playwright to check that document.querySelectorAll('.project-card').length === projects.length.

3) Launch configuration validation
- Open .vscode/launch.json in VS Code; ensure JSON parses and contains cwd exactly "${workspaceFolder}/app" in at least one configuration.
- Run the configuration (if the environment supports it) to confirm it launches a browser to the configured URL. This step is optional.

4) Functional manual test steps
- Start server: cd app && python3 -m http.server 8000
- Open http://localhost:8000/index.html
- Confirm:
  - Page header shows "Project Pulse" or agreed heading.
  - Project cards appear matching the 3 sample items from project-data.json.
  - Each card shows name, owner, status, recentActivity, priority.
  - Styling is applied (cards show background/border).
  - Open devtools console: ensure no fetch or JSON parse errors.

Automated checks to add later (suggestions)
- JSON schema + ajv for CI: add a JSON schema that requires the "projects" array and the five fields; add a script to validate project-data.json during CI.
- Simple headless browser test (Puppeteer/Playwright) to validate DOM: ensure .dashboard exists and number of .project-card elements matches JSON length.
- Linting: HTML validation and CSSlint in CI.

Potential risks / edge cases and mitigations
1) Fetch fails when previewed via file://
   - Mitigation: Document clearly that an HTTP server must be used. Provide commands (python3 -m http.server or npx http-server).

2) Malformed or missing project-data.json
   - Mitigation: Add defensive code in index.html to show a friendly error message if fetch fails or JSON lacks "projects". In QA, validate JSON before rendering.

3) Missing fields in some project objects
   - Mitigation: Renderer should provide safe fallbacks (e.g., display "—" or "Unknown" when a field is missing). Tests should assert presence or fallback.

4) Inconsistent priority/status values
   - Mitigation: Document expected enums (e.g., status ∈ {"on track","at risk","blocked"}, priority ∈ {"low","medium","high"}). Not strictly enforced now but useful for future validation.

5) Accessibility and contrast issues
   - Mitigation: Use accessible color contrasts and semantic markup (headings, roles). Designer to run a quick contrast check.

6) Browser compatibility for fetch
   - Mitigation: Modern browsers support fetch. If older browsers need support, a tiny polyfill could be added later. For now target modern browsers.

Validation expectations (explicit checklist)
- JSON:
  - [ ] app/project-data.json exists.
  - [ ] JSON top-level property "projects" exists and is an array.
  - [ ] Each project has name, owner, status, recentActivity, priority.
- HTML:
  - [ ] app/index.html exists.
  - [ ] HTML links to app/styles.css.
  - [ ] There is an element with class "dashboard".
  - [ ] The script renders a .project-card for each project.
- CSS:
  - [ ] app/styles.css exists.
  - [ ] Contains .dashboard selector.
  - [ ] Contains .project-card selector.
- VS Code config:
  - [ ] .vscode/launch.json exists.
  - [ ] It is valid JSON.
  - [ ] It contains cwd exactly set to "${workspaceFolder}/app".
- Runtime:
  - [ ] The page renders correctly when served with a static server (python or npx http-server).
  - [ ] No console errors on load.

Open questions (document here so Orchestrator can resolve)
- Should recentActivity be an ISO 8601 timestamp or a human-friendly string? (Plan uses ISO 8601 but will accept either — decide).
- Do we need to support priority as numeric or textual values in the tests? (Plan uses textual "low"/"medium"/"high".)
- Should the renderer do client-side sorting/filtering? (Not in scope for minimal implementation.)
- Any corporate branding/colors or accessible contrast requirements that must be enforced now?

Checklist for the Orchestrator / Coder / Designer before marking done
- [ ] app/project-data.json created and validated (3 sample projects included).
- [ ] app/index.html created, links to styles.css, fetches project-data.json, renders .project-card elements into .dashboard.
- [ ] app/styles.css created with .dashboard and .project-card selectors and minimal responsive styling.
- [ ] .vscode/launch.json created with cwd = "${workspaceFolder}/app" and valid JSON.
- [ ] Manual run: cd app && python3 -m http.server 8000 → open browser → confirm rendering and styling.
- [ ] No console errors (fetch/parse errors) on load.
- [ ] docs/project-pulse-plan.md saved with this plan.

Notes for the implementer (practical tips)
- Keep markup simple and semantic. Tests will query by class names, not complex selectors.
- Use fetch('/project-data.json') or relative path fetch('project-data.json') in index.html. When previewing from app/, both work if the server's working dir is app/.
- Keep CSS specificity low so tests can reliably target .dashboard and .project-card.
- If implementing fallback content for empty projects, ensure .dashboard still exists even when empty (helps tests).

Final: Save this plan file
- File to create: docs/project-pulse-plan.md
- Copy this full content into docs/project-pulse-plan.md for repository documentation.

If you want, I can now:
- Generate example contents for each of the four required files (app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json) as minimal ready-to-drop-in files (still static, no build). Tell me “Generate files” and I will produce the exact file contents for human copy/paste.