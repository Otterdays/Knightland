<p align="center">
  <img src="https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust&logoColor=white" alt="Rust" />
  <img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black" alt="React" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white" alt="Vite" />
  <img src="https://img.shields.io/badge/PixiJS-66A3E0?style=flat-square&logo=pixi.js&logoColor=white" alt="PixiJS" />
  <img src="https://img.shields.io/badge/SpacetimeDB-1a1a2e?style=flat-square" alt="SpacetimeDB" />
</p>

<h1 align="center">⚔️ Knightland</h1>
<p align="center"><strong>A cross-platform top-down pixel MMORPG</strong></p>
<p align="center">Real-time multiplayer • Server-authoritative • One codebase, web to desktop</p>

---

## The Citadel Stack

Knightland is built on a unified architecture we call **The Citadel** — no separate game server, no sync layer bolted on. The backend *is* the database. The client *is* the renderer. Everything flows through one source of truth.

| Component | Codename | Tech |
|-----------|----------|------|
| **Backend** | Project RoundTable | SpacetimeDB + Rust |
| **Renderer** | Excalibur Engine | PixiJS v8 |
| **UI** | Heraldry Interface | Vite + React + TypeScript |
| **Sync** | Knight's Pulse | SpacetimeDB SDK |
| **Desktop** | Fortress Containers | Tauri (Phase 2) |

```
┌─────────────────────────────────────────────────────────────┐
│  Heraldry Interface (React)  │  Excalibur Engine (PixiJS)   │
│  HUD • Menus • Inventory     │  World • Sprites • Camera    │
└────────────────────┬──────────────────────────────────────┘
                      │ Knight's Pulse (SpacetimeDB SDK)
                      ▼
┌─────────────────────────────────────────────────────────────┐
│              Project RoundTable (SpacetimeDB)               │
│         Database + Game Server + Reducers (Rust)           │
└─────────────────────────────────────────────────────────────┘
```

---

## Status

**Phase:** Planning / Architecture finalized

- [x] Architecture & stack decisions
- [x] Knightland rebrand & Citadel codenames
- [ ] **Phase 1:** Core foundations (RoundTable + Excalibur + Heraldry)
- [ ] **Phase 2:** Movement & Knight's Pulse
- [ ] **Phase 3:** Combat & Inventories
- [ ] **Phase 4:** Desktop/Mobile (Fortress Containers)

---

## Quick Links

| Doc | Purpose |
|-----|---------|
| [Architecture](DOCS/ARCHITECTURE.md) | System design, data flow, directory layout |
| [Roadmap](DOCS/ROADMAP.md) | The Quest — full feature roadmap |
| [SBOM](DOCS/SBOM.md) | Security & dependency tracking |
| [Changelog](DOCS/CHANGELOG.md) | Version history |

---

## Why the Citadel Stack?

- **Project RoundTable (SpacetimeDB):** Database and game server in one. Real-time by default. No REST, no polling.
- **Excalibur Engine (PixiJS):** High-performance 2D rendering for pixel art. WebGL-first.
- **Heraldry Interface (React):** Mature UI ecosystem. Hooks, state, and tooling that scale.
- **Fortress Containers (Tauri):** Rust-powered desktop builds. Small binaries, native feel.

---

## License

*TBD*
