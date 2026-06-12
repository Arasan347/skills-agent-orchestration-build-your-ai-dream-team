---
title: Project Pulse — Implementation Plan
---

# Project Pulse — Implementation Plan

Date: 2026-06-12  
Owner: Orchestrator (plan). Implementers: Designer, Coder

## Executive summary

- Project Pulse is a lightweight static dashboard that surfaces high-level project status across multiple projects (health, progress, next milestone, owners). Goal: create a clean, accessible, responsive single-page dashboard that reads a local JSON file (app/project-data.json) and displays cards/rows, summary KPIs, and a filter/search. Minimal dependencies; easy to run locally via a static server or VS Code launch.

## Scope & deliverables

Deliverables (explicit files — files to create or modify):
- app/index.html — (Coder) main dashboard HTML and inline or linked script to load/apply JSON data.
- app/styles.css — (Coder + Designer) visual styles, CSS variables, responsive rules.
- app/project-data.json — (Coder) sample data file with 3 sample projects (see "Optional content" below).
- .vscode/launch.json — (Coder) VS Code debug/serve configuration for Local Server or Live Server integration.
- docs/project-pulse-plan.md — (Planner) this file (to be added to repo).
- Optional (recommended): app/main.js — (Coder) modular JavaScript to fetch JSON and render DOM (if not using inline script).
- Optional assets folder: app/assets/ (Designer) icons or illustrations (SVG/PNG).

## Roles & responsibilities

### Designer (UI/UX, assets, accessibility)
- Deliver:
  - Visual mock (can be a simple annotated screenshot) and style token list.
  - Assets: SVG icon set (status dot, progress, filter/search, owner avatar placeholders) saved in app/assets/.
  - Accessibility checklist: color contrast report for primary palette, keyboard tab order flow, ARIA role suggestions for dynamic elements, focus styles.
  - Responsive breakpoints and behaviors.
- Specific tasks & files (owner):
  - app/styles.css — propose color palette, typography scale, spacing scale and example classes (Designer supplies tokens and example CSS snippets; Coder implements).
  - app/index.html — propose semantic layout (header, nav/filter, main, card list), section IDs and ARIA roles to be used by Coder.
  - app/assets/*.svg or *.png — provide optimized assets (SVG preferred).
  - docs/project-pulse-plan.md — confirm UI/UX decisions referenced in the plan.
- UI/UX decisions to provide:
  - Primary / accent / neutral colors (hex), accessible contrast pairings (AA/AAA targets).
  - Type scale (base font-size, h1–h4 sizes).
  - Spacing tokens (xs/s/m/l/xl).
  - Button styles, input styles, focus outlines.
  - Responsive breakpoints (mobile-first): 0–599px (mobile), 600–899px (tablet), 900px+ (desktop).
- Accessibility checks:
  - Color contrast >= 4.5:1 for body text, >=3:1 for large text.
  - All interactive elements keyboard accessible (tab order), visible focus states.
  - ARIA labels for the search, filters, and each project card role="article" with aria-labelledby.
- Estimated effort: Medium (4–8 hours)

### Coder (HTML, CSS, JSON, VS Code config, wiring data)
- Deliver:
  - app/index.html — semantic structure, link to app/styles.css, minimal script tag or app/main.js to fetch project-data.json and render DOM.
  - app/styles.css — implement Designer's tokens, layout, responsive rules, accessible focus.
  - app/project-data.json — valid JSON with sample projects (3) and schema comment in docs (see below).
  - .vscode/launch.json — configuration to launch a simple static server and open browser to localhost.
  - app/main.js (optional) — discrete JS module to fetch and mount data; fallback to inline script in index.html if preferred.
- Specific tasks & files (owner):
  - Create/modify: app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json, optional app/main.js, optional app/assets (icons from Designer).
- Implementation details:
  - HTML: semantic header (id="pulse-header"), controls/form (id="pulse-controls"), main (id="pulse-main"), container for KPIs (id="pulse-kpis"), project list (id="project-list"). Use data-* attributes for templating.
  - CSS: CSS variables (root) for tokens; mobile-first responsive rules and grid/list views.
  - Data wiring: fetch('/app/project-data.json') or relative path; graceful fallback and error UI (id="data-error").
  - Local serving: explain why static server recommended (fetch fails on file://).
- Estimated effort: Medium (6–12 hours)

## Dependencies

Runtime & dev dependencies (recommended)
- Static server (choose one):
  - Node.js + http-server (simple): node >= 14.17, http-server >= 14.0.0 (install: npm i -g http-server)
  - OR VS Code Live Server extension (ritwickdey.liveserver) — recommended for rapid dev.
- VS Code:
  - VS Code >= 1.60
  - Recommended extensions: Live Server, Prettier (optional), ESLint (optional if creating JS).
- Optional npm packages (if adding build):
  - None required for pure static site. If using a dev bundler:
    - esbuild or Vite (node >= 16 recommended). But NOTE: not required.
- Browser support:
  - Modern evergreen browsers (Chrome, Edge, Firefox, Safari). Use fetch API — polyfill if you need to support legacy IE.
- Project type:
  - Purely static site (no build) — recommended. Use a local static server for fetch access. If team prefers bundler, add Vite as a follow-up.

## Work breakdown and phases

### Phase 1 — Design (UI/UX & tokens)
- Time: 4–8 hours
- Tasks (Designer):
  - Create mock of the dashboard (mobile + desktop). [Medium — 3–5h]
  - Define color, type, spacing tokens and provide CSS variable list. [Low — 1–2h]
  - Create and optimize SVG assets and placeholder avatars into app/assets/. [Low — 1–2h]
  - Produce accessibility checklist and responsive breakpoints. [Low — 0.5–1h]
- Files impacted:
  - app/styles.css (draft tokens/snippets), app/index.html (proposed structure), app/assets/*

### Phase 2 — Implementation (HTML/CSS/JSON/Serve config)
- Time: 6–12 hours
- Tasks (Coder):
  - Create app/project-data.json (sample + schema). [Low — 0.5–1h]
  - Implement app/index.html with semantic sections and script hook points. [Medium — 2–4h]
  - Implement app/styles.css with variables, layout, responsive rules, accessible focus. [Medium — 2–4h]
  - Create .vscode/launch.json config to start Live Server or a Node script to serve app/. [Low — 0.5–1h]
  - Implement app/main.js or inline fetch/render code and error handling. [Medium — 1–2h]
- Files impacted:
  - app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json, optional app/main.js

### Phase 3 — Integration (wire up assets, data, and accessibility)
- Time: 3–6 hours
- Tasks (Designer + Coder):
  - Integrate Designer assets into markup and styles. [Low — 0.5–1h]
  - Verify ARIA attributes and keyboard navigation. [Low — 0.5–1h]
  - Final visual polish and responsive checks on sample data. [Medium — 1–2h]
- Files impacted:
  - app/index.html, app/styles.css, app/assets/*

### Phase 4 — Validation & Documentation
- Time: 2–4 hours
- Tasks:
  - Run manual checks and document acceptance criteria (this plan includes checks). [Low — 0.5–1h]
  - Add brief README or comments in index.html explaining how to run with Live Server. [Low — 0.5–1h]
  - Fix issues discovered. [Medium — 1–2h]
- Files impacted:
  - docs/project-pulse-plan.md (this plan), app/index.html (comments), optionally README.md

## Parallel work decisions

Safe parallel tasks (minimal merge conflict risk)
- Designer can simultaneously produce visual mock, CSS tokens, and assets in app/assets/ while Coder scaffolds HTML and basic styles.
- File-level parallelism:
  - Designer: modifies/proposes app/styles.css (tokens and samples) and creates app/assets/*.
  - Coder: creates app/index.html and app/project-data.json and .vscode/launch.json.
- Notes:
  - To avoid merge conflicts on app/styles.css, split responsibilities:
    - Designer creates file fragment tokens named app/styles.tokens.css (or a comment block at top) or share token spec in docs; Coder integrates tokens into final app/styles.css. This prevents editing same lines.
  - If both edit app/index.html, coordinate sections: Designer edits top-of-file comment and a "design-notes" block; Coder owns the DOM structure and script hooks.

Sequential tasks (must be done in order)
- Implementing dynamic rendering (Coder) requires app/project-data.json to exist (or sample stub).
- Accessibility verification should happen after HTML + CSS are in place.
- Final visual polish after assets are integrated.

## Edge cases to handle

- project-data.json missing or malformed — show friendly error (id="data-error") and fallback skeleton UI.
- Empty projects array — show empty state with CTA to add projects.
- Long text (project names, notes) — truncate with ellipsis and reveal full text on hover or via details modal.
- Offline / file:// loading — fetch will fail when opening via file://; make it explicit to use static server.
- Accessibility pitfalls — color contrast insufficient on chosen palette; ensure tokens meet contrast targets.
- Large number of projects — ensure list is scrollable and uses a vertical list; consider virtualization if >200 items (out of scope).
- Mobile layout compressed — stacked cards should be readable and controls accessible.

## Validation expectations and acceptance criteria

How to run:
- Start local server:
  - Option A: In project root, run: npx http-server -c-1 -p 8080 (requires Node) and open http://localhost:8080/app/index.html
  - Option B: Use VS Code Live Server extension; use provided .vscode/launch.json to open the server.
- Or open via VS Code debug: use .vscode/launch.json command to start Live Server or run the above npm command.
- Manual checks: open the page on desktop and mobile (responsive mode) and verify items below.

### Functional validation (pass/fail)
- Fetch & render:
  - Pass: The page loads and displays KPIs and 3 sample project cards from app/project-data.json within 3s on local machine.
  - Fail: No data shown and no useful error message.
- Filtering/search:
  - Pass: Search box filters visible cards in real-time (case-insensitive). Filter controls (status) toggle visible cards.
  - Fail: Filters do not affect list or cause console errors.

### Visual validation (pass/fail)
- Layout:
  - Pass: Header, controls, KPIs, and project list appear in specified order and reflow correctly at breakpoints.
- Styles:
  - Pass: CSS tokens applied; spacing and typography match mock within reasonable tolerance.
- Fail: Major layout break (overlapping elements, unreadable text).

### Accessibility (pass/fail)
- Color contrast:
  - Pass: Body text >= 4.5:1; large text >= 3:1. Designer provided palette documented.
- Keyboard navigation:
  - Pass: Tab order reaches search, filters, each project card action (if any), and provides visible focus styles.
- ARIA:
  - Pass: Search has aria-label; each project card has role="article" and aria-labelledby.
- Fail: Any interactive element not reachable via keyboard, or color contrast below thresholds.

### Performance
- Pass: Initial load (assets + JSON) < 1s on local dev environment; minimal CSS and SVGs optimized.
- Fail: Large unoptimized images or blocking scripts causing major delays.

### Automated checks (optional)
- Linting:
  - CSS: run through stylelint (optional).
  - HTML: use htmlhint (optional).
- Basic unit: small script tests for JSON shape (optional; not required for static).

## Integration notes and handoff checklist

Pre-merge checklist:
- Designer confirms final colors and assets are committed to app/assets/.
- Coder confirms app/index.html fetches app/project-data.json using a relative path and that error handling exists.
- Run manual checks on both mobile and desktop breakpoints.
- Add small comment in index.html explaining how to start the server and open page.

Merge & conflict resolution guidance:
- Avoid simultaneous edits to the same lines in app/styles.css. If conflicts occur:
  - Prefer Designer tokens as source-of-truth for colors/spacing.
  - Rebase and manually reconcile token definitions, preserving CSS variable names.
- If both modified app/index.html, use semantic blocks to resolve conflicts: keep Coder's script hooks and adopt Designer's structural suggestions where possible.

Final verification (post-merge):
- Pull latest, run static server, open /app/index.html, run acceptance criteria above.
- Confirm .vscode/launch.json works in VS Code (open workspace, run the configured launch).

## Optional content (examples & suggestions)

### Example app/project-data.json (sample content)
- Coder to commit this exact JSON file (3 sample projects):

```json
{
  "projects": [
    {
      "id": "proj-001",
      "name": "Website Redesign",
      "owner": "Asha N.",
      "status": "At Risk",
      "progress": 62,
      "next_milestone": "Content Freeze",
      "due_date": "2026-07-15",
      "notes": "Dependency: Marketing approval pending."
    },
    {
      "id": "proj-002",
      "name": "Mobile App v2",
      "owner": "Diego R.",
      "status": "On Track",
      "progress": 28,
      "next_milestone": "Beta Release",
      "due_date": "2026-09-01",
      "notes": "Sprint 5 in progress."
    },
    {
      "id": "proj-003",
      "name": "Analytics Pipeline",
      "owner": "Priya S.",
      "status": "Complete",
      "progress": 100,
      "next_milestone": "N/A",
      "due_date": "2026-05-30",
      "notes": "Deployed to staging; monitor for 2 weeks."
    }
  ]
}
```

### Suggested structure for app/index.html (IDs and classes)
- Header and controls:
  - header#pulse-header
    - h1#pulse-title
    - div#pulse-controls
      - input#pulse-search[type="search" aria-label="Search projects"]
      - div#pulse-filters (buttons with data-filter attributes)
- KPIs:
  - section#pulse-kpis (contains .kpi-card elements)
- Project list:
  - main#pulse-main
    - ul#project-list (role="list")
      - li.project-item[role="listitem"] > article.project-card[role="article" aria-labelledby="project-title-{id}"]
        - h2#project-title-{id}
        - p.project-meta
        - div.project-progress (progress element or ARIA-enhanced bar)
- Error / empty states:
  - div#data-error[hidden] — visible on fetch errors
  - div#empty-state[hidden] — when no projects

### Suggested CSS variables (to implement in app/styles.css)

```css
:root {
  --bg: #FFFFFF;
  --surface: #F6F7F9;
  --text: #17202A;
  --muted: #52606D;
  --accent: #2563EB; /* primary */
  --danger: #DC2626;
  --success: #10B981;
  --kpi-bg: #FFFFFF;
  --card-radius: 8px;
  --gap-xs: 4px;
  --gap-s: 8px;
  --gap-m: 16px;
  --gap-l: 24px;
  --font-sans: "Inter", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  --font-size-base: 16px;
}
```

### .vscode/launch.json (suggested content)
- Provide a launch config that opens Live Server or runs a task to start http-server. Example:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Project Pulse (Live Server)",
      "type": "pwa-msedge",
      "request": "launch",
      "url": "http://127.0.0.1:5500/app/index.html",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

(Adjust port 5500 if Live Server uses another port. Alternatively add a task that runs "npx http-server ./ -p 8080" and attach to it.)

## Assumptions

- Repository currently has an app/ directory; this plan adds app/index.html, app/styles.css, app/project-data.json and .vscode/launch.json (and optional app/main.js, app/assets/) without changing other code.
- Team prefers a static, no-build approach. If a JS bundler is required later, adjust dependencies.
- Browser environment will be modern (fetch + ES6 supported). If older support needed, polyfills will be added later.
- Designer and Coder have shared access to the repo and will coordinate token ownership to avoid style merge conflicts.

## Open questions

1. Do we want a separate JS file (app/main.js) or is inline script inside index.html acceptable?
2. Should we support sort and advanced filtering (tags, owner) in v1, or keep v1 to search + status filter only?
3. Is there a preferred color palette or brand token set already in the org repo to align with?
4. Is image hosting required for avatars or will we use inline SVG placeholders?
5. Should we add automated tests (linting, JSON schema validation) as part of CI — if so, which CI system will run them?

## Acceptance & next steps

- Designer: produce mock, token list, and assets; push assets to app/assets/ and a token snippet to app/styles.tokens.css or a design notes block in docs.
- Coder: scaffold index.html, styles.css (hooked to tokens), project-data.json with sample content, and .vscode/launch.json. Implement fetch + render and error handling.
- After both push, run validation checklist, resolve conflicts per "Integration notes", and merge.

## Estimated total effort
- Designer: 4–8 hours
- Coder: 6–12 hours
- Integration & validation: 2–4 hours

End of plan.
