# Implementation Plan: Knightland Foundations ⚔️

This plan details the immediate next steps to bring the **Citadel Stack** to life, focusing on Phase 1 of the Quest.

## 🏗️ Objectives
1.  **Initialize Project RoundTable:** Set up the SpacetimeDB Rust module.
2.  **Scaffold Heraldry Interface:** Create the Vite + React frontend.
3.  **Establish Knight's Pulse:** Link the frontend to the backend via the TS SDK.
4.  **Ignite Excalibur Engine:** Ready the PixiJS v8 renderer for the first draw call.

---

## 🛠️ Step-by-Step Execution

### 1. The Core (Project RoundTable)
- **Action:** Create `server/` directory and initialize SpacetimeDB.
- **Tools:** `spacetime init --lang rust server`
- **First Table:** Define `Knight` (formerly Player) with `identity`, `nickname`, and `pos: Vec2`.
- **First Reducer:** `register(nickname: String)` to spawn a new knight.

### 2. The Window (Heraldry Interface)
- **Action:** Scaffold `client/` using Vite and the Vinext core principles.
- **Dependencies:** `react`, `react-dom`, `pixi.js`, `spacetimedb-sdk`.
- **Structure:** Set up the folder hierarchy as defined in [ARCHITECTURE.md](file:///c:/Users/motor/Desktop/something_crazy/DOCS/ARCHITECTURE.md).

### 3. The Pulse (Synchronization)
- **Action:** Generate TypeScript bindings from the server module.
- **Command:** `spacetime generate --lang typescript --out-dir client/src/spacetime/bindings`
- **Hook:** Create `useKnightPulse` for real-time state updates.

### 4. The Blade (Excalibur Engine)
- **Action:** Initialize the PixiJS Application.
- **Component:** `GameView.tsx` to mount the canvas.
- **Test:** Render a single placeholder pixel-art square moving via server-validated coordinates.

---

## 👁️ Observability Path (The Oracle)
From day one, we will include tracing hooks:
- **SpacetimeDB side:** Log every reducer execution time.
- **Client side:** Track "Time to First Render" and "WebSocket Latency".

---

## 📝 Success Criteria
- [ ] Server starts and accepts connections.
- [ ] Client loads and displays a login screen.
- [ ] A "Knight" appears on the map after registration.
- [ ] No syntax errors in the auto-generated bindings.
