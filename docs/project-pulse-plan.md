# Project Pulse implementation plan

## Summary

Build a lightweight static dashboard that lets Mona's contributors scan active projects, ownership, status, recent activity, priority or risk, and a short summary. Keep the implementation dependency-free and serve it over HTTP from `app/`, so the page can load `project-data.json` and the launch configuration opens the dashboard rather than a directory listing.

## Responsibilities

- **Orchestrator:** Confirm the shared markup and data contract below, assign the non-overlapping files, coordinate parallel implementation, then integrate and verify the result.
- **Planner:** Define the implementation phases, dependencies, file ownership, edge cases, and validation criteria; surface any uncertainty rather than silently changing the brief.
- **Designer:** Own the visual and responsive design in `app/styles.css`. Establish a clear information hierarchy, accessible contrast and focus states, readable status and priority badges, and polished project cards. Use `.dashboard` and `.project-card` hooks and coordinate selectors with the agreed markup contract.
- **Coder:** Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Implement semantic dashboard markup and rendering from the JSON data, provide understandable loading/empty/error states, and configure a deterministic launch that serves `app/` and opens `index.html`.

## Shared implementation contract

- `app/project-data.json` has a top-level `projects` array. Each entry includes the required `name`, `owner`, `status`, `recentActivity`, and `priority` fields. Include a concise `summary` field as well, so the contributor-friendly summary in the brief is represented in the data rather than hard-coded in the page.
- Agree on shared class hooks before implementation: `.dashboard` for the main dashboard region and `.project-card` for each project. Use semantic elements and stable hooks for status and priority styling.
- The app loads the JSON over HTTP from the same `app/` directory. The launch configuration must set its working directory to `${workspaceFolder}/app` and open `index.html`; do not rely on opening the HTML through `file://`.
- Keep all launch settings in strict JSON without comments. Do not add dependencies or extra application files unless the Orchestrator approves a change to scope.

## Ordered phases and file assignments

1. **Set the contract — Orchestrator and Planner.** Confirm the data fields, summary addition, markup hooks, status/priority presentation, and launch behavior above. No application files are assigned in this phase.
2. **Implement in parallel after the contract is agreed.**
   - **Designer:** `app/styles.css` — create the responsive dashboard styling against `.dashboard`, `.project-card`, and the agreed status/priority hooks. Do not edit the HTML or data.
   - **Coder:** `app/index.html`, `app/project-data.json`, `.vscode/launch.json` — create the semantic page, sample project records, data loading/rendering, and launch configuration. Keep selectors aligned with the shared contract; do not edit the stylesheet.
3. **Integrate and validate — Orchestrator coordinates; Designer and Coder fix their owned files.** Check the rendered page against the brief, resolve any markup/style contract mismatch through the owner of the affected file, and verify launch behavior.

## Dependencies and parallel work decisions

- Phase 2 depends on Phase 1: both specialists need the agreed data and selector contract before working independently.
- Within Phase 2, Designer and Coder can work in parallel because their file assignments do not overlap. Their work is only independent if the shared hooks and intended content hierarchy are fixed first.
- Integration and launch validation depend on both Phase 2 workstreams being complete. Any correction stays with the owner of the file being changed.
- The Coder should establish the JSON shape and representative records before wiring the HTML renderer to them. The launch configuration can be authored in that same workstream, but final launch validation depends on the page and data being present.

## Edge cases and risks

- No project records: show a friendly empty state rather than a blank dashboard.
- Data request fails or JSON is malformed: show a clear error state; do not render a success-shaped empty result.
- Missing or unfamiliar status/priority values: keep the text visible and readable, and use a safe neutral visual treatment rather than failing to render a card.
- Long project names, summaries, or activity text: allow wrapping without clipping cards or breaking the responsive layout.
- Narrow viewport, keyboard navigation, and assistive technology: preserve semantic structure, visible focus, sufficient contrast, and status meaning that is not conveyed by color alone.
- Loading JSON via `file://` will not behave like an HTTP-served app in many browsers; validate through the launch configuration or another local HTTP server.

## Validation expectations

- Confirm all four assigned files exist and `app/project-data.json` and `.vscode/launch.json` parse as strict JSON.
- Check the data has a top-level `projects` array and each sample record contains `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
- Inspect `app/index.html` and the stylesheet for the `.dashboard` and `.project-card` hooks, and confirm cards expose the required project fields and summary.
- Run **Run Project Pulse Dashboard** from VS Code. Verify the server serves from `app/`, the browser opens `index.html`, and the JSON request succeeds without a directory listing or console errors.
- In the browser, verify populated and empty/error states, status and priority treatments, responsive card layout at narrow widths, keyboard focus, and readable contrast.
