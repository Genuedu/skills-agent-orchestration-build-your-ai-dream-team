# Project Pulse implementation plan

## Summary

Build Mona's Project Pulse as a lightweight, responsive static dashboard that lets contributors quickly see each project's name, owner, status, recent activity, priority or risk, and a short contributor-friendly summary. Use semantic HTML, CSS, and JSON; avoid introducing a framework or build system. The page should load project data over HTTP and render readable project cards, with a VS Code launch configuration that starts a local server from `app/` and opens the dashboard page.

## Team responsibilities

- **Planner** researches the repository and records the file ownership, dependencies, sequence, risks, and validation below.
- **Designer** provides the visual and accessibility direction: information hierarchy, card and badge treatment, contrast, typography, spacing, keyboard/readability considerations, and responsive behavior. Share those decisions with Coder before implementation; avoid editing Coder-owned files to prevent conflicts.
- **Coder** implements and validates the assigned HTML, CSS, JSON, and launch configuration, staying within the file scope below.
- **Orchestrator** assigns the design brief and coding work, integrates the handoff, checks acceptance criteria, and reports any remaining risks.

## File assignments

| File | Owner | Scope |
| --- | --- | --- |
| `app/project-data.json` | Coder | Create a strict JSON document with a top-level `projects` array. Each project must have `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short contributor-friendly `summary` as well. Use a few representative, realistic projects and consistent, documented-in-plan status/priority values. |
| `app/index.html` | Coder | Create the semantic page titled **Project Pulse**, link `styles.css`, and load `project-data.json`. Render visible `.project-card` elements from the data, showing the required project fields and summary. Include accessible headings/labels and a clear, human-readable loading or failure state. Keep any small rendering script inline so the requested deliverable stays a three-file static app. |
| `app/styles.css` | Coder, informed by Designer | Implement the Designer's direction with responsive layout, readable spacing and contrast, status/priority badges, and polished cards. Include `.dashboard` and `.project-card` selectors, `border-radius`, and `box-shadow`; ensure small-screen layouts remain usable. |
| `.vscode/launch.json` | Coder | Add strict JSON configuration named **Run Project Pulse Dashboard**. Use `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`, so launch opens the page rather than the directory listing. |
| `docs/project-pulse-plan.md` | Planner | This implementation plan only. Do not edit application or agent-definition files as part of planning. |

## Ordered implementation and dependencies

1. **Design direction — Designer.** Define the first-view hierarchy, card content order, status/priority distinction, accessibility and responsive expectations. This is an input to coding, not a separate source file.
2. **Data contract and sample content — Coder.** Create `app/project-data.json` with the required fields and a top-level array. Agree on a small consistent set of status/priority strings so the UI can display them predictably.
3. **Dashboard structure and styling — Coder.** Create `app/index.html` and `app/styles.css`, applying the Designer's brief. The page depends on the data contract from step 2; its markup and styles should share class names and render all required fields.
4. **Run configuration — Coder.** Create `.vscode/launch.json` with the specified server working directory, command, and page URL. It depends on the page existing at `app/index.html`.
5. **Integration and acceptance — Orchestrator with Coder.** Run the checks below, start the configured launch, and confirm the rendered page. Fix any integration gaps within the assigned files and report results.

### Parallel versus sequential work

- Designer's visual/accessibility brief and Coder's initial JSON data can proceed **in parallel** because the brief does not depend on sample project values. Coder should settle the field names and display categories with the Orchestrator early and send the contract to Designer; Designer should design against the required fields rather than inventing a competing schema.
- Coder should wait for the design brief before finalizing presentation, then implement HTML and CSS together. Data may be authored in parallel with the brief, but HTML's rendering logic depends on the JSON field contract.
- The launch file can be drafted in parallel with page styling once the `app/` directory and expected `index.html` target are agreed, but final run verification must happen **after** all four files exist.
- Validation and launch smoke testing are sequential after implementation; they verify the integrated result, not isolated agent deliverables.

## Dependencies and implementation notes

- There are no application package dependencies or build step in the repository. Use browser-native HTML/CSS/JavaScript and Python's standard-library `http.server` for preview.
- Loading JSON with `fetch` requires HTTP; opening `index.html` directly as a `file://` URL may block the request. The launch configuration must serve the `app/` directory and open `index.html`.
- Render supplied project strings as text rather than injecting them as HTML. Preserve the required `recentActivity` field name in JSON while presenting its value in contributor-friendly wording.
- Keep status and priority labels readable without relying on color alone. Include visible loading and fetch-error feedback so an empty or unavailable data response is not mistaken for a successful dashboard.

## Validation expectations

1. Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
2. Run `python3 -m json.tool app/project-data.json >/dev/null` and `python3 -m json.tool .vscode/launch.json >/dev/null`; both must pass strict JSON parsing.
3. Inspect the app files for the required contract: page title `Project Pulse`; links to `styles.css` and `project-data.json`; `.project-card` markup that visibly includes project cards and each project's status, `recentActivity`, and priority; top-level `projects` data with `name`, `owner`, `status`, `recentActivity`, and `priority`; and `.dashboard`, `.project-card`, `border-radius`, and `box-shadow` CSS hooks.
4. Confirm launch JSON includes `Run Project Pulse Dashboard`, `python3 -m http.server 5500`, `${workspaceFolder}/app`, `serverReadyAction`, and `http://localhost:%s/index.html`.
5. Start **Run Project Pulse Dashboard** in VS Code. Verify the browser opens the dashboard UI—not a directory listing—cards populate from JSON, key information is readable, and the layout remains usable at a narrow viewport. Check the loading/error message behavior if feasible, then stop the server.
6. Run `scripts/validate-exercise.sh` if available in the learner environment as a broader repository regression check. The Step 3 workflow checks file presence, required phrases/hooks, JSON syntax, and launch naming/target; it does not replace the manual browser smoke test.

## Risks and open questions

- The brief does not prescribe exact sample project names, statuses, or priorities. Use a small, clearly fictional or representative dataset and consistent values; confirm the team wants no external/live API before adding any integration (none is needed for this static exercise).
- The launch configuration assumes Python 3 is available on PATH and that port `5500` is free. If the launch fails due to a port conflict or environment-specific VS Code debugger support, diagnose that first and preserve the required app working directory and index URL when adapting the launch setup.
- The static page depends on serving `app/` over HTTP for JSON loading. Testing only by opening the HTML file directly is not a valid end-to-end check.
- Workflow checks are primarily textual and can pass despite runtime/data-rendering defects. Treat the VS Code browser smoke test and manual accessibility/responsive review as required acceptance checks.
- No additional files are in scope. If implementation discovers a need for a separate JavaScript module, framework, or dependency, ask the Orchestrator to revise scope rather than silently expanding the deliverables.
