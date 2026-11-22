# Project Pulse Implementation Plan

## Goal

Build a small, polished, static Project Pulse dashboard for Mona's contributors. The dashboard must make active projects, owners, status, recent activity, priority or risk, and short summaries easy to scan in a responsive card-based layout.

The implementation is limited to these deliverables:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

No other application or tooling files should be needed.

## Ownership and responsibilities

### Orchestrator

- Confirm the requested outcome and the stable cross-file contract before implementation.
- Give the Designer and Coder explicit file scopes and prevent overlapping edits.
- Sequence work at file and dependency boundaries.
- Integrate the outputs, resolve conflicts, and perform the final validation.
- Do not create or edit implementation files; the work is delegated to specialists.
- Do not stage, commit, or push unless Mona explicitly requests it.

### Designer

- Own the visual and accessibility direction for `app/styles.css`.
- Define the dashboard information hierarchy, project-card layout, status and priority treatments, typography, spacing, border radius, shadows, responsive behavior, and visible focus states.
- Include the deterministic hooks `.dashboard` and `.project-card`.
- Prefer semantic HTML and sufficient text/background contrast over decorative effects.
- Use the HTML contract for card fields and statuses, but do not edit `app/index.html`, `app/project-data.json`, or `.vscode/launch.json`.
- Report design decisions and validation recommendations; do not commit or push.

### Coder

- Implement `app/index.html`, `app/project-data.json`, and `.vscode/launch.json` within the assigned scope.
- Use `app/index.html` as the exact visible page title **Project Pulse**.
- Reference `styles.css` and `project-data.json` from `app/index.html`.
- Parse and render the top-level `projects` array with visible project cards, including `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Give each rendered project card the exact class `project-card`.
- Use deterministic, descriptive markup and a clear DOM structure that Designer's CSS can style.
- Create strict JSON in `.vscode/launch.json` with no comments.
- Configure the launch name **Run Project Pulse Dashboard**, serve with `python3 -m http.server 5500` from `${workspaceFolder}/app`, and use `serverReadyAction` to open `http://localhost:%s/index.html`.
- Do not edit `app/styles.css` unless the Designer explicitly delegates a small, non-overlapping markup or stylesheet correction.
- Report changed files, validation results, and risks; do not commit or push.

## Implementation phases

### Phase 1: Establish the shared contract

The Orchestrator reviews the brief and existing repository structure, then fixes the implementation contract before delegation:

- `app/project-data.json` has exactly one top-level key, `projects`.
- Each project object contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Initial data should use clear representative values, such as multiple active, planned, or at-risk projects and a priority of low, medium, high, or critical.
- `app/index.html` contains the exact title text **Project Pulse**, loads `styles.css`, loads or fetches `project-data.json`, and renders `.project-card` elements.
- `app/styles.css` defines `.dashboard` and `.project-card`; card, status, priority, owner, and activity regions use stable class names agreed with the HTML.
- The dashboard assumes it is served over HTTP, not opened through a `file://` URL.

The contract is the dependency for all specialist implementation work.

### Phase 2: Parallel implementation

After the contract is established, independent work can run in parallel:

- **Designer,** assigned only `app/styles.css`, creates the polished dashboard styles and visual state treatments.
- **Coder,** assigned `app/project-data.json` and `.vscode/launch.json`, creates valid project data and the deterministic preview configuration.
- **Coder,** assigned `app/index.html`, creates the accessible page shell and project-card rendering logic.

The Coder may work on `app/index.html`, `app/project-data.json`, and `.vscode/launch.json` together because those files do not overlap. The Designer and Coder may work at the same time because `app/styles.css` is a separate file scope. HTML and CSS are coordinated only through the phase 1 class-name contract.

This parallel phase is safe provided no agent edits outside its assigned files.

### Phase 3: Integration and first-view verification

The Orchestrator:

- Reviews all four deliverables as an integrated result.
- Confirms that `index.html` references `styles.css` and project data correctly.
- Confirms that CSS hooks and markup class names match.
- Checks that all required fields are represented visually.
- Confirms that launch settings serve `app/` and open `index.html`, not a server directory listing.
- Resolves only conflicts caused by the shared contract.

Any correction should be assigned back to the owner of the affected file. It must not move ownership of `app/styles.css` to the Coder unless a targeted CSS fix was explicitly assigned.

### Phase 4: Validation and handoff

The Orchestrator runs the validation below, summarizes participating agents and outcomes, and records any remaining limitation in the final handoff. The dashboard work remains uncommitted unless Mona separately instructs otherwise.

## Dependencies

- `app/index.html` depends on the final field names in `app/project-data.json` and the visual hooks in `app/styles.css`.
- `app/styles.css` depends on the HTML structure and stable class names agreed in the shared contract.
- Project-card rendering depends on valid JSON and the top-level `projects` array.
- Launch behavior depends on `app/index.html` existing and being the target at `http://localhost:%s/index.html`.
- Final validation depends on all four files existing and on the launch preview using the `app/` working directory.
- No implementation file should be created or changed until the Orchestrator has assigned it an explicit owner.

## Parallel work decisions

Parallel:

- Designer may create `app/styles.css` while Coder creates `app/project-data.json`.
- Coder may create `app/index.html` and `.vscode/launch.json` concurrently after the shared contract is fixed.
- Independent static-file validation may occur after the relevant files exist, but integration validation must wait for all four.

Sequential:

- Repository and brief review precedes any implementation.
- The shared HTML/CSS/data contract must precede parallel implementation.
- `app/index.html` rendering, `app/styles.css`, and `app/project-data.json` integration follows specialist work.
- Launch validation follows creation of both `.vscode/launch.json` and `app/index.html`.
- The final validation and handoff follow integration.

## Validation expectations

### File and syntax validation

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Parse `app/project-data.json` with `python3 -m json.tool` and verify a top-level `projects` array.
- Verify every project object contains `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Verify `app/index.html` contains the exact title text **Project Pulse**.
- Verify `app/index.html` references `styles.css` and loads or fetches `project-data.json`.
- Verify visible project-card markup uses `project-card` and that all required fields are rendered.
- Parse `.vscode/launch.json` as strict JSON and confirm no comments.
- Confirm the launch is named **Run Project Pulse Dashboard**.
- Confirm `cwd` is `${workspaceFolder}/app`, the command is `python3 -m http.server 5500`, and `serverReadyAction` opens `http://localhost:%s/index.html`.

### CSS and accessibility validation

- Confirm `.dashboard` and `.project-card` selectors exist.
- Confirm styling includes border radius and box shadow and a readable, responsive card layout.
- Confirm status badges and priority treatment are visually distinct and have clear text labels, not color alone.
- Confirm heading, project-name, owner, status, activity, and priority hierarchy is readable.
- Check keyboard focus, semantic markup, link/button behavior if present, and sufficient contrast.
- Check the first viewport at desktop and narrow widths; cards should reflow rather than overflow.

### Runtime validation

- Start **Run Project Pulse Dashboard** from VS Code and confirm it serves port 5500 from `app/`.
- Confirm the browser opens `http://localhost:%s/index.html` and shows the Project Pulse dashboard, not directory listings or a blank page.
- Confirm cards and every required data field appear after loading the JSON.
- Stop the preview server after verification.

## Edge cases

- **No projects:** render a clear, accessible empty state rather than a blank dashboard.
- **Malformed or unavailable JSON:** show an explicit, useful error in the page and do not leave silently broken card markup.
- **Missing project fields:** do not render `undefined` or `[object Object]`; hide or label missing values consistently.
- **Long names or activity text:** wrap text, constrain cards sensibly, and prevent horizontal page overflow.
- **Status and priority casing:** normalize display styling so minor case differences do not break badges.
- **Unknown status or priority:** provide neutral fallback styling instead of omitting the value.
- **Narrow or zoomed viewports:** reflow cards and controls; preserve readable spacing and tap or focus targets.
- **Launching from VS Code:** use `cwd` to avoid serving the repository root and use `serverReadyAction` to open the HTML route explicitly.
- **Static data loading:** serve over the configured HTTP server because browser fetch behavior for local JSON may differ when opened directly.

## Definition of done

Project Pulse is complete when the static dashboard is visible and readable, all project fields are sourced from `app/project-data.json`, the card and dashboard styles are polished and responsive, the VS Code launch opens `app/index.html` directly, and both structural and manual validation have passed.
