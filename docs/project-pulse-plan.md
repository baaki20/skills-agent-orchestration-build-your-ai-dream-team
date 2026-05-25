# Project Pulse — Implementation Plan

## 1) Summary
Create a small, iterative, and fully-runnable Project Pulse dashboard inside the repository's app/ folder. Goal is to deliver a working static dashboard quickly (HTML + JSON + minimal JS) and then incrementally improve responsive styling, accessibility, and simple interactivity (search/filter). The Designer owns deterministic CSS hooks and visual/responsive polish; the Coder implements the HTML structure, reads static project-data.json, wires minimal JS rendering and creates a VS Code launch configuration that runs the site from app/index.html with cwd = ${workspaceFolder}/app. Plan emphasizes small phases, clear file ownership, and parallel work where file scopes don't overlap.

---

## 2) Phases

Phases are ordered for a quick working prototype first, then refinements.

Phase 1 — Minimal runnable dashboard (skeleton)
- Goal
  - Produce a runnable dashboard showing cards for each project loaded from app/project-data.json. Provide deterministic CSS hooks so Designer can style without changing HTML later. Provide .vscode/launch.json that launches the app with cwd = ${workspaceFolder}/app and opens index.html.
- Files created/modified (ownership)
  - Coder:
    - app/index.html (creates basic semantic HTML, container elements with classes: .dashboard, .dashboard-header, .projects-grid, .project-card)
    - app/project-data.json (create minimal seed JSON with 3 sample projects containing id, name, status, owner, percent_complete, updated_at)
    - .vscode/launch.json (create launch config that launches index.html with cwd set to ${workspaceFolder}/app)
    - app/main.js (optional small script to fetch project-data.json and render cards; can be inline in index.html if preferred)
  - Designer:
    - app/styles.css (create an empty scaffold with CSS class names / comments and basic font stack, plus a small placeholder to ensure styles are loaded; include deterministic hooks like .dashboard, .project-card, .project-title, .project-status)
- Designer tasks
  - Provide class names and a simple CSS scaffold with comments describing intended states (.project-card--overdue, .project-status--green, etc.)
  - Verify visual layout wireframe to match HTML structure.
- Coder tasks
  - Implement HTML structure and minimal JS to load project-data.json and render project cards using the deterministic class hooks.
  - Create .vscode/launch.json (cwd: ${workspaceFolder}/app, open index.html).
- Estimated effort
  - Small (3–6 hours)

Phase 2 — Designer: Full visual & responsive styling
- Goal
  - Implement styling for desktop, tablet, mobile; ensure color contrast and responsive grid for .projects-grid. Provide explicit focus and keyboard-visible states.
- Files created/modified (ownership)
  - Designer:
    - app/styles.css (populate with full styles; media queries; utility classes; clear comments describing hook usage)
  - Coder:
    - app/index.html (minor adjustments if the Designer requests additional semantic elements or attributes; align the DOM to CSS hooks)
- Designer tasks
  - Produce the full CSS: typography scale, spacing system, responsive grid, color tokens, accessible focus outlines, and state classes (.project-card--highlight, .project-status--attention).
  - Deliver accessible CSS class naming and examples in comments (how to indicate priority/status).
- Coder tasks
  - Make minimal HTML edits if needed to align to CSS hooks (e.g., add data- attributes or additional wrapper elements).
- Estimated effort
  - Medium (6–12 hours)

Phase 3 — Data enrichment & realistic sample data
- Goal
  - Expand project-data.json into a richer sample dataset and make sure the UI handles different values (long titles, missing owner, 0%/100% completion).
- Files created/modified (ownership)
  - Coder:
    - app/project-data.json (enrich with 8–12 sample projects; fields: id, name, description, owner {name,email}, status, percent_complete, tags[], updated_at, link)
    - app/main.js (if needed) — ensure rendering supports new fields (description truncation, tags)
  - Designer:
    - app/styles.css (add styles for description, tags, avatar placeholder, and truncated text)
- Designer tasks
  - Provide styles for tags, owner avatar placeholder, and a card layout variant when description is long.
- Coder tasks
  - Update renderer to handle optional fields (owner missing, tags empty) without errors and to sanitize long text (truncate with title attribute).
- Estimated effort
  - Small (3–6 hours)

Phase 4 — Simple interactivity: search + status filter
- Goal
  - Add a small search box and status filter (All / Active / At-risk / Complete) in the header. Wire them with minimal, accessible JS so the table filters client-side.
- Files created/modified (ownership)
  - Coder:
    - app/index.html (add search input and filter controls with deterministic classes: .dashboard-search, .dashboard-filter)
    - app/main.js (implement search and filter logic; keep it small and dependency-free)
  - Designer:
    - app/styles.css (style search and filter controls; focus states; responsive behavior)
- Designer tasks
  - Style the controls to be visible and accessible, making sure layout works on small screens.
- Coder tasks
  - Implement accessible keyboard behavior (Enter to search) and debounce for search (small debounce).
  - Ensure filtering is case-insensitive and matches project name, owner, tags.
- Estimated effort
  - Medium (4–8 hours)

Phase 5 — Accessibility audit and polish
- Goal
  - Add ARIA labels, ensure tabindex order, confirm color contrast and keyboard navigation, fix corner-case UI bugs, and produce a short test checklist for maintainers.
- Files created/modified (ownership)
  - Designer:
    - app/styles.css (final focus styles; reduced-motion preferences; high-contrast adjustments)
  - Coder:
    - app/index.html (add aria-* attributes and semantic roles; update markup where necessary)
    - app/main.js (improvements for keyboard support and screen-reader announcements)
- Designer tasks
  - Perform an accessibility pass and produce a short list of changes (e.g., "ensure .dashboard-search has aria-label").
- Coder tasks
  - Implement small JS improvements to announce filter results (aria-live region) and ensure focus is handled after actions.
- Estimated effort
  - Small (3–6 hours)

Optional Phase 6 — Minor enhancements / QA pass
- Goal
  - Bug fixes, cross-browser checks, small animations (prefers-reduced-motion aware), and integration notes for future enhancements (server data, pagination).
- Files created/modified
  - app/styles.css, app/main.js, app/project-data.json as required.
- Estimated effort
  - Small (2–4 hours)

---

## 3) Dependencies
- Phase 1 must complete before Phase 2 and Phase 4 (Phase 2 needs HTML hooks; Phase 4 needs search/filter DOM elements and data).
- Phase 3 depends on Phase 1 (project-data.json exists and renderer supports fields) but can run in parallel with Phase 2 after the initial skeleton is available (Designer styling and Coder data enrichment won't conflict if responsibilities are followed).
- Phase 4 depends on Phase 1 and Phase 3 (renderer + data present).
- Phase 5 depends on Phase 2–4 to audit final markup, styles and behavior.
- Optional Phase 6 depends on previous phases for final polish.

---

## 4) Parallelization decisions
- Phase 1 (skeleton) — must be done first because HTML structure & launch config are the base.
- After Phase 1:
  - Phase 2 (styling) and Phase 3 (data enrichment) can run in parallel:
    - Why: Designer edits app/styles.css while Coder edits app/project-data.json and app/main.js. The file scopes are different. Coder and Designer must coordinate on any small DOM changes, but those are limited and planned.
  - Phase 4 (interactivity) can begin after Phase 1 and as soon as Phase 3 provides enough data — partial work is possible (Coder can build filter logic using the initial JSON).
- Phase 5 (accessibility & polish) should run only after Phase 2–4 are functionally complete (sequential).
- Optional Phase 6 can run in parallel with Phase 5 for non-conflicting tweaks (e.g., performance vs. a11y testing).

---

## 5) Validation expectations

General launch instructions (mandatory for all phases)
- How to run via VS Code:
  - Open the workspace in VS Code / Codespaces.
  - Press F5 (Run). Select the launch configuration named e.g., "Run Project Pulse" created in .vscode/launch.json.
  - The launch config must have:
    - cwd: ${workspaceFolder}/app
    - file: ${workspaceFolder}/app/index.html
  - The browser should open index.html. If running as file:// is unreliable, use the Live Server extension or a small static server, but the launch config must still open index.html with cwd = app.
- Manual alternative:
  - Open app/index.html in a browser (file:// path) or run a simple static server from the app folder and open http://localhost:PORT/index.html.

Phase 1 validation (Skeleton)
- Open index.html via the launch config.
- Expected:
  - A header with the dashboard title.
  - A grid showing 3 project "cards" rendered from project-data.json (each card shows title, status, owner, percent).
  - CSS should be loading (basic scaffold visible) and the DOM must include elements with hooks: .dashboard, .projects-grid, .project-card, .project-title, .project-status.
- Checks:
  - Open DevTools → Console: no JSON parsing or runtime errors.
  - View page source: index.html contains minimal script tag linking to project-data.json or main.js.
  - Confirm .vscode/launch.json exists and content includes cwd reference.

Phase 2 validation (Styling)
- Open index.html.
- Expected:
  - Responsive grid: multiple columns on desktop, stack on small screens.
  - Visible focus outlines when tabbing through interactive elements.
  - Color contrast adheres to WCAG (high-level check with contrast tools).
  - The Designer's CSS hooks should be present in the DOM and selectors should apply correctly.
- Checks:
  - Resize the browser to confirm breakpoint behavior.
  - Use keyboard only (Tab) to navigate to at least the search field or the first card; focus should be visible.

Phase 3 validation (Data)
- Open index.html.
- Expected:
  - 8–12 sample projects rendered.
  - UI gracefully handles long titles (truncation with title tooltip), missing owners (show placeholder), and 0%/100% completion visuals.
- Checks:
  - Inspect different cards for expected fields and verify no errors in console.
  - Verify tags are rendered and truncation behavior exists.

Phase 4 validation (Search & Filter)
- Open index.html.
- Expected:
  - A search input with class .dashboard-search that filters cards live (or on Enter).
  - Status filter controls (.dashboard-filter) filter by status.
  - Filtering is case-insensitive and accessible (labels, role=toolbar if multiple controls).
- Checks:
  - Type a project name fragment and confirm displayed cards reduce to matching items.
  - Click filter and verify results update and screen-reader announcements appear (if implemented).
  - No unexpected console errors.

Phase 5 validation (Accessibility)
- Open index.html.
- Expected:
  - aria-* attributes present where needed: aria-label on search, aria-live region for results, roles for controls.
  - Keyboard navigation works (Tab/Shift+Tab/Enter).
  - Reduced motion preference respected (no forced animations if prefers-reduced-motion).
- Checks:
  - Run a basic axe or manual a11y checks: check for missing alt text (images), focusable elements with visible focus, and semantic headings.
  - Try with a keyboard and confirm you can open and use filters and read card content.

Validation notes for the Orchestrator
- The Orchestrator should run each stage by launching via .vscode/launch.json and opening the page. The acceptance checks above should be executed manually or by a simple smoke test script (if desired later).

---

## 6) Open questions and risks

Open questions
- Preferred browser for the launch config? (pwa-chrome vs pwa-msedge) — specify which extension is expected in the environment; otherwise use the "file" launch to default browser.
- Do we need images/avatars or is an initial avatar placeholder sufficient? (Impacts asset handling).
- Should the search/filter state be reflected in the URL (deep-linking) or is client-only state acceptable initially?
- Is there a plan to add server-side data later? If yes, we should keep renderer modular.

Risks
- Using file:// to open index.html in Chrome can block some scripts or CORS behavior; mitigate by recommending Live Server or launching Chrome with file access (but plan requires the VS Code launch config to set cwd and open index.html).
- Potential merge conflicts if Designer and Coder edit the same file (index.html or styles.css) simultaneously. Mitigation: clearly assign lines of ownership (Designer to own styles.css; Coder to own DOM class names and structure in index.html).
- Missing a11y attributes initially — must be explicitly added in Phase 5.
- Long text and layout edge cases not covered by initial styling; will need to be handled in Phase 3–5.
- If the Codespace runs without GUI browser support, the launch config may not open — provide fallback: open index.html manually on a local machine.

---

## 7) Deliverable checklist (files and acceptance criteria)

Deliverables that the Orchestrator can assign and verify:

Phase 1 deliverables
- Files:
  - app/index.html — contains header, .dashboard container, .projects-grid, and initial DOM hooks (.project-card, .project-title, .project-status).
  - app/project-data.json — contains at least 3 sample projects (id, name, status, owner, percent_complete, updated_at).
  - .vscode/launch.json — contains a launch configuration named "Run Project Pulse" with cwd: ${workspaceFolder}/app and file: ${workspaceFolder}/app/index.html (or equivalent to open index.html).
  - app/styles.css — scaffold with CSS class names and at least a minimal rule to confirm loading (body font-family).
  - app/main.js (or inline script in index.html) — small script that fetches project-data.json and renders the project cards.
- Acceptance criteria:
  - Running the launch config opens index.html and displays at least 3 project cards.
  - No JSON parsing errors in console.
  - DOM contains the deterministic CSS hooks listed above.

Phase 2 deliverables
- Files:
  - app/styles.css — fully-populated responsive stylesheet with comments for hooks, media queries for breakpoints, focus outlines, and state classes (.project-card--overdue, .project-status--green, etc.)
  - (Possible minor) app/index.html updates for extra semantic wrappers if Designer requests.
- Acceptance criteria:
  - Visual design applied: card spacing, responsive grid, visible focus states, and good color contrast.
  - DOM class names remain the same or are updated in small, deliberate ways agreed between Coder and Designer.

Phase 3 deliverables
- Files:
  - app/project-data.json — enriched sample dataset (8–12 entries; fields: id, name, description, owner, status, percent_complete, tags[], updated_at, link).
  - app/main.js — renderer updated to handle optional/missing fields and long text safely.
  - app/styles.css — styles for tags, description, and placeholder avatar.
- Acceptance criteria:
  - UI renders all sample items; long text is truncated with title attribute; missing owner shows placeholder.
  - No runtime errors.

Phase 4 deliverables
- Files:
  - app/index.html — addition of search and filter controls with deterministic classes (.dashboard-search, .dashboard-filter).
  - app/main.js — implementation of client-side search & filter logic.
  - app/styles.css — styles for controls (responsive and accessible).
- Acceptance criteria:
  - Search box filters results (case-insensitive). Status filter updates results.
  - Keyboard operable; no runtime errors; results count updates if implemented.

Phase 5 deliverables
- Files:
  - app/index.html — ARIA attributes and roles added as required.
  - app/main.js — aria-live announcements and keyboard improvements.
  - app/styles.css — final focus and reduced-motion handling.
- Acceptance criteria:
  - Basic a11y checks pass (keyboard navigation, aria-labels present for controls, visible focus).
  - Reduced motion respected and no blocking accessibility issues.

Final deliverable: docs/project-pulse-plan.md
- This file (the current document) must be committed to docs/project-pulse-plan.md and used as the single source of truth for phases, file ownership, and validation steps.

---

Notes for Orchestrator assignment
- Assign Phase 1 first to the Coder and Designer concurrently but with clear file boundaries: Coder must create index.html, project-data.json, .vscode/launch.json and main.js; Designer creates styles.css scaffold. The Orchestrator should ensure the Coder pushes the skeleton before the Designer begins full styles to avoid mismatches.
- Provide the Coder and Designer with the deterministic class name list to avoid friction.
- Keep each phase small and confirm acceptance criteria before moving to the next phase.
