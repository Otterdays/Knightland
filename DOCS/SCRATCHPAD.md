<!-- PRESERVATION RULE: Never delete or replace content. Append or annotate only. -->

## 2026-03-18 - README & Docs
- Created user-facing `README.md` for GitHub: Citadel Stack overview, architecture diagram,
  status, quick links, tech badges (Rust, React, TS, Vite, PixiJS, SpacetimeDB).
- Updated CHANGELOG, SUMMARY.

## 2026-03-18 - Git Prep
- Initialized git repo.
- Added `.gitignore` (Node, Rust, Vite, Tauri, SpacetimeDB, IDE, OS, logs).
- Created `debugs/.gitkeep` (debug logs gitignored per workflow).

## 2026-03-12 - Roadmap Enhancement Checkpoint
- Added roadmap backlog items for threat modeling, env/secret policy, dependency
governance, Windows launch scripts, server runbooks, performance budgets, soak tests,
contract tests, and Playwright coverage.
- Noted stack drift: `project_structure.md`, `implementation_plan_v1.md`, and
`opus4.6_crossplatform-stuff.md` still reference `Vinext` while the current stack points
to `Vite + React`.
- Missing core doc: `DOCS/STYLE_GUIDE.md` is referenced by workflow but does not exist yet.

## Active Tasks
- [ ] Initialize **Project RoundTable** (server/ module)
- [ ] Scaffold **Heraldry Interface** (Vite + React)
- [ ] Establish **Knight's Pulse** (SpacetimeDB SDK connection)

## Blocked Items
None

## Recent Context (last 5 actions)
1. Expanded **ROADMAP.md** with 'more bits and pieces' (Observability, Content Pipeline, Audio, DevOps, Markets).
2. Reviewed project documentation setup to build deep domain context.
3. Officially rebranded project to **Knightland**.
4. Assigned "Top Secret" cool names: **Project RoundTable**, **Excalibur Engine**, **Heraldry Interface**, **Fortress Containers**, **Knight's Pulse**.
5. Finalized tech stack decision: Vite + React + SpacetimeDB + PixiJS.

## Compacted History
*(Initial project initialization 2026-03-12)*

## Key Decisions
- **Project Name: Knightland**
- **The Citadel Stack:** SpacetimeDB + Rust + PixiJS + React + Tauri.
- **Phase-based Roadmap:** Web first, then desktop, then mobile.
- **Cool Names:** Use specialized terminology for internal components to keep the "Game Dev" mindset alive.
