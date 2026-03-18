## 2026-03-12 - Initializing "Knightland" 🛡️
### Current Understanding
The user has officially named the game **Knightland**. We are moving away from generic titles. The goal is to build a top-down pixel MMORPG with a "Game Developer" mindset, prioritizing UX and fun.

### Options Considered
- Use generic names (Backend, Engine).
- Use thematic names (Project RoundTable, Excalibur Engine).

### Decision & Rationale
**Adopted the Citadel Stack terminology.** This fits the "Knightland" theme and creates a stronger project identity. 
- **Project RoundTable:** SpacetimeDB/Rust Backend.
- **Excalibur Engine:** PixiJS Renderer.
- **Heraldry Interface:** React UI.
- **Knight's Pulse:** State Sync logic.
- **Fortress Containers:** Tauri/Capacitor shells.

The roadmap is now phased as "The Quest for the Realm," giving us clear, flexible milestones. We are starting with the foundations: The RoundTable and Heraldry Interface.

---

## 2026-03-12 - Vinext vs Vite + React — Stack Decision
### Decision & Rationale
**Confirmed Vite + React.** Vinext is too experimental for a complex game like Knightland. We need the stability of Vite and the official support of SpacetimeDB's React templates to ship Knightland efficiently.
