## System Overview: Knightland ⚔️
A cross-platform top-down pixel MMORPG built using the **Citadel Stack**.

The architecture is built on four pillars:
1. **The RoundTable (Backend):** SpacetimeDB is the entire backend. It is both the database AND the game server. Logic lives in **Rust modules**.
2. **Excalibur Engine (Renderer):** A custom PixiJS v8 world renderer for lightning-fast pixel-art mapping and sprites.
3. **Heraldry Interface (UI shell):** A Vite + React SPA that handles HUDs, menus, and inventory.
4. **Knight's Pulse (Sync):** The SpacetimeDB SDK that keeps all players in a single, fluid state.
5. **The Oracle (Observability):** OpenTelemetry + Arize integration for real-time realm monitoring.
6. **The Sky-Watcher (Atmospherics):** Global state management for weather and day/night cycles.

---

## The Citadel Directory Structure

```text
/
├── server/                 # Project RoundTable (Rust)
│   ├── src/
│   │   ├── lib.rs          # Main entry point
│   │   ├── tables/         # Knight entities, Position, Inventory
│   │   ├── reducers/       # Combat, Movement, Login logic
│   │   └── utils/          # Math helpers
│   ├── Cargo.toml          # Rust dependencies
│   └── .spacetime/         # SpacetimeDB project config
│
├── client/                 # Knightland Client (Vite + React)
│   ├── src/
│   │   ├── main.tsx        # Heraldry Entry
│   │   ├── engine/         # Excalibur Engine (PixiJS loop)
│   │   ├── spacetime/      # Knight's Pulse (Auto-generated bindings)
│   │   ├── hooks/          # React hooks (useKnight, useWorld)
│   │   ├── components/     # Heraldry UI (HUD, Chat)
│   │   └── assets/         # Sprites and Tiles
│   ├── package.json
│   └── vite.config.ts
│
├── desktop/                # Fortress Container (Tauri Phase 2)
│   ├── src-tauri/
│   └── package.json
│
├── DOCS/                   # The Scribe's Archives
│   ├── ARCHITECTURE.md     # Current file
│   ├── ROADMAP.md          # The Quest
│   └── ...
└── README.md
```

---

## Knight's Flow (Data Flow)

1. **Intention:** A player inputs a command via the `Heraldry Interface`.
2. **Action:** The `Excalibur Engine` identifies the need for a state change and triggers `Knight's Pulse`.
3. **Transcendence:** The command is sent to `The RoundTable` (SpacetimeDB).
4. **Judgment:** Rust reducers validate the move and mutate the world state.
5. **Revelation:** SpacetimeDB broadcasts the update.
6. **Manifestation:** All clients receive the update via `Knight's Pulse`, and the `Excalibur Engine` renders the new reality.

---

## Cross-Platform Strategy: Fortress Containers

| Platform         | Method                | Status     |
|------------------|-----------------------|------------|
| Web (Standard)   | Clean Vite SPA        | ✅ Ready    |
| Windows Desktop  | Fortress (Tauri)      | 🟡 Phase 2 |
| Mobile           | Fortress (Capacitor)  | 🟡 Phase 3 |

---

## Why the Citadel Stack?

- **Project RoundTable (SpacetimeDB):** No separate server/DB mess. Real-time by default.
- **Excalibur Engine (PixiJS):** High-performance 2D rendering.
- **Heraldry Interface (React):** Best-in-class UI state management.
- **Fortress Containers (Tauri):** Rust-powered lightweight desktop apps.
