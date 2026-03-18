# Knightland Roadmap — "The Quest for the Realm"

## 2026-03-12 - Roadmap Enhancement Pass

These are the missing guardrails and delivery tools that should be planned early so later
phases do not hide avoidable risk.

### A. Security, Access, and Recovery

- [ ] **Threat model pass:** Map trust boundaries across browser client, desktop wrapper,
SpacetimeDB module, admin tools, and asset CDN. Capture abuse cases for cheating, spam,
impersonation, leaked secrets, and compromised admin accounts.
- [ ] **Environment contract:** Add `.env.example` files and boot-time validation for
required values such as module name, host URL, asset base URL, and telemetry endpoint.
Fail fast on startup if config is missing or malformed.
- [ ] **Session storage policy:** If external auth is introduced, prefer HttpOnly cookies
or platform secure storage. Do not rely on `localStorage` for privileged session secrets.
- [ ] **Secret scanning:** Add `gitleaks` (or equivalent) to CI and pre-commit checks to
block leaked keys, tokens, and accidental `.env` commits.
- [ ] **Admin audit + restore drills:** Record admin-only reducer activity in an
`AdminAuditLog`, pair it with snapshot backups, and test restore/rollback procedures on a
schedule before live content tools ship.

### B. Dependency and Tooling Baseline

- [ ] **Package manager decision:** Standardize on `bun` or `npm`, commit one lockfile,
and align setup docs, scripts, and CI with that choice.
- [ ] **Frontend tooling baseline:** Add planning for `typescript-eslint`,
`eslint-config-prettier`, `@vitest/coverage-v8`, `@playwright/test`, `msw`, `zod`, and
`vite-tsconfig-paths` so validation, testing, and path aliasing are covered from day one.
- [ ] **Rust dependency policy:** Add `cargo-deny` alongside `cargo audit` to enforce
license and advisory rules. Mirror approved dependency policy in `SBOM.md`.
- [ ] **Dependency update cadence:** Schedule weekly bot update PRs, monthly manual review
of major releases, and a changelog skim before merge for critical runtime packages.
- [ ] **Stack source-of-truth cleanup:** Remove remaining `Vinext` references and add the
missing `DOCS/STYLE_GUIDE.md` so the chosen `Vite + React` stack and conventions are
documented in one place.
- [ ] **Testing suite architecture:** Design test pyramid: 70% unit tests (Vitest), 20% integration tests (MSW + Vitest), 10% E2E (Playwright). Add `test:unit`, `test:integration`, `test:e2e`, `test:all` scripts.
- [ ] **Test coverage enforcement:** Set coverage thresholds: 80% line coverage, 70% branch coverage. Fail CI if coverage drops below threshold. Use `@vitest/coverage-v8` with `coverage.provider = 'v8'`.
- [ ] **Mocking strategy:** Use `msw` (Mock Service Worker) to intercept SpacetimeDB SDK calls in tests. Create `src/mocks/handlers.ts` with handlers for all reducers and table subscriptions.
- [ ] **Snapshot testing:** Use `vitest-snapshot` for JSON responses, dialogue trees, and item definitions. Review snapshots in PRs to catch unintended schema changes.
- [ ] **Mutation testing:** Add `mutate` or `stryker-mutator` to Rust tests to verify test effectiveness by introducing intentional bugs and confirming tests catch them.
- [ ] **Fuzz testing:** Add `cargo-fuzz` for reducers that process user input (chat messages, item names). Catch panics and assertion failures from malformed inputs.
- [ ] **Visual regression testing:** Integrate `playwright-screenshots` or `chromatic` for UI components. Store baseline screenshots in `__snapshots__/visual/`. Fail build on visual diff > 1%.

### C. Server Management and Environments

- [ ] **One-command local launch:** Add Windows-friendly `launch.bat` and `scripts/dev.ps1`
to start SpacetimeDB, publish the module, generate bindings, and boot the client in the
correct order.
- [ ] **Environment matrix:** Define `local`, `dev`, `staging`, and `prod` module names,
seed data expectations, and promotion gates before shared testing begins.
- [ ] **Health verification script:** Add a smoke check that confirms the server is
reachable, the module is published, bindings are current, and client config points at the
expected environment.
- [ ] **Operational runbooks:** Document start, stop, restart, crash recovery, logs,
snapshots, restore, and schema migration rollback steps before first external playtest.
- [ ] **Structured logs + trace IDs:** Standardize logs across server, client, and desktop
wrapper so multiplayer bugs can be followed from input to reducer to render path.
- [ ] **Server management CLI:** Build a custom `knightctl` CLI tool using `clap` (Rust). Commands: `knightctl status`, `knightctl restart-module`, `knightctl rollback`, `knightctl backup`, `knightctl metrics`.
- [ ] **Container orchestration:** Create `docker-compose.yml` for local dev environment. Include SpacetimeDB, Redis (caching), Grafana (metrics), Jaeger (tracing). Use `docker compose up -d` to launch full stack.
- [ ] **Configuration management:** Use `config.toml` for server settings (not hardcoded). Include: log level, tick rate, world bounds, feature flags. Hot-reload via `SIGHUP` without restarting.
- [ ] **Server metrics dashboard:** Deploy Grafana with pre-built dashboards for: active connections, reducer latency (p50/p95/p99), memory usage, database size, error rate by type.
- [ ] **Alerting rules:** Configure alerts in Grafana/PagerDuty for: reducer latency > 100ms, connections > 80% capacity, error rate > 1%, memory > 90%.
- [ ] **Log aggregation:** Use Loki or ELK stack for centralized logging. Index by: timestamp, identity, reducer name, zone. CreateSavedFilters for common queries (player issues, error patterns).
- [ ] **Backup strategy:** Automated daily snapshots of SpacetimeDB module data. Retain: 7 daily, 4 weekly, 12 monthly. Test restore procedure quarterly.
- [ ] **Disaster recovery plan:** Document RTO (Recovery Time Objective) < 1 hour, RPO (Recovery Point Objective) < 24 hours. Include failover steps if primary region goes down.
- [ ] **Server-side modding API:** Expose admin endpoints for spawning NPCs, triggering events, modifying terrain. Use JWT authentication with short expiry. Log all admin actions.

### D. Performance, Stability, and Asset Budgets

- [ ] **Target hardware matrix:** Pick minimum-spec desktop, laptop, and mobile targets,
plus supported browsers, so performance work has a real baseline.
- [ ] **Performance budget table:** Track FPS, memory ceiling, WebSocket RTT, chunk-load
time, first-play load time, bundle size, and texture memory budget by platform.
- [ ] **Profiling toolkit:** Plan a baseline workflow using browser performance tools,
PixiJS frame stats, `criterion` for Rust hot paths, and `k6` for concurrent session tests.
- [ ] **Long-session soak tests:** Run 30-60 minute idle and movement sessions to catch
memory leaks, reconnect loops, stale subscriptions, and object-pool churn.
- [ ] **Asset budget enforcement:** Set limits for atlas size, audio bitrate, total
preloaded asset weight, and loading regressions so content growth stays measurable.

### E. Testing Matrix and Suites

- [ ] **Reducer correctness tests:** Add Rust unit tests plus `proptest` coverage for
movement, combat math, inventory invariants, and rate-limit behavior.
- [ ] **Schema contract tests:** Verify generated TypeScript bindings stay in sync with the
published Rust schema before merges land.
- [ ] **Client/server smoke suite:** Add a local integration flow for register -> spawn ->
move -> chat -> reconnect to validate the main loop against a real SpacetimeDB instance.
- [ ] **Playwright E2E coverage:** Plan browser flows for login, movement, settings
persistence, chat, reconnect behavior, and desktop-wrapper smoke checks.
- [ ] **Visual regression checks:** Use screenshot assertions for HUD panels, menus, and a
representative game scene to catch UI and Pixi rendering regressions.
- [ ] **Integration test suite:** Create `tests/integration/` with Vitest. Test: reducer → database → subscription → client update flow. Use testcontainers or local SpacetimeDB instance.
- [ ] **Property-based testing:** Add `proptest` crate for Rust. Generate 1000+ random inputs for movement calculations, combat formulas, inventory operations. Catch edge cases.
- [ ] **Chaos testing:** Introduce random failures (network latency, disconnects, reducer errors) in test environment. Verify client handles gracefully (reconnect, queue actions, show errors).
- [ ] **Load test suite:** Run k6 scripts as part of CI on nightly builds. Track: p99 latency, error rate, memory growth. Fail build if metrics degrade > 10% from baseline.
- [ ] **A/B test framework:** Implement `Experiment` table for toggling features per-player. Track metrics in analytics. Use for tuning combat balance, economy rates, tutorial effectiveness.
- [ ] **Synthetic monitoring:** Deploy Playwright tests to check production environments every 5 minutes. Verify: login flow, character spawn, movement, chat send. Alert on failures.

This roadmap outlines the journey to build **Knightland**. Each item is a concrete, implementable technical task — not a vague goal.

---

## 🛡️ PHASE 1: Foundations of the Citadel
*Goal: A single knight can connect, appear on a canvas, and move — validated by the server.*

### 1.1 Project RoundTable — SpacetimeDB Rust Module

- [ ] **`spacetime init --lang rust server`** — Scaffold the Rust module. Confirm `Cargo.toml` pulls `spacetimedb = "1.x"`.
- [ ] **`Knight` table:** Define the primary entity with `#[table(name = knight, public)]`. Fields: `identity: Identity`, `nickname: String`, `x: f32`, `y: f32`, `chunk_x: i32`, `chunk_y: i32`, `hp: u32`, `max_hp: u32`, `level: u32`.
- [ ] **`Session` table:** `#[table(name = session)]` with `connection_id: ConnectionId`, `knight_id: Identity`, `last_heartbeat: Timestamp`. Used to detect dropped clients.
- [ ] **`register(nickname: String)` reducer:** Validate name length (2–20 chars), assert identity doesn't already exist, insert into `Knight` at default spawn `(x: 64.0, y: 64.0)`.
- [ ] **`connect` / `disconnect` lifecycle reducers:** Use `#[reducer(client_connected)]` and `#[reducer(client_disconnected)]` hooks to create/clean up `Session` rows and zero out stale state.
- [ ] **`constants.rs`:** Define `TILE_SIZE: f32 = 16.0`, `CHUNK_SIZE: u32 = 32`, `TICK_RATE: u64 = 20`, `SPAWN_X/Y: f32`, shared across all reducers.
- [ ] **`spacetime publish knightland-dev`** — Deploy module to local SpacetimeDB instance for first smoke test.

### 1.2 Heraldry Interface — Vite + React + TypeScript

- [ ] **`npm create vite@latest client -- --template react-ts`** — Scaffold the client.
- [ ] **Install core deps:** `@clockworklabs/spacetimedb-sdk`, `pixi.js@^8`, `zustand` (global store), `react-router-dom@^6`.
- [ ] **`spacetime generate --lang typescript --out-dir client/src/spacetime/bindings`** — Auto-generate table types and reducer wrappers from the Rust module. Re-run on every Rust schema change.
- [ ] **`spacetime/client.ts`:** Initialize `SpacetimeDBClient`, manage auth token via `localStorage`, handle reconnect with exponential backoff (cap at 30s).
- [ ] **`useKnightPulse.ts` hook:** Subscribe to the `Knight` table filtered by `ctx.identity`, expose `.knight` and `.isConnected`. Use `DbConnection.onConnect` / `onDisconnect` callbacks.
- [ ] **`useNearbyKnights.ts` hook:** Subscribe to `Knight` table using `SELECT * FROM knight WHERE chunk_x = ? AND chunk_y = ?`. Refresh subscription on chunk change.
- [ ] **`vite.config.ts`:** Configure path aliases (`@/` → `src/`), add `@vitejs/plugin-react-swc` for faster HMR.
- [ ] **Testing foundation:** Set up `vitest` with `@testing-library/react`. Create `src/__tests__/hooks.spec.ts` for hook unit tests. Configure `jsdom` for DOM testing.
- [ ] **Code quality:** Install `eslint`, `prettier`, `eslint-plugin-react`, `eslint-plugin-react-hooks`. Create `.eslintrc.js` and `.prettierrc`. Add `lint` and `format` scripts to `package.json`.
- [ ] **Dependency management:** Add `npm audit` and `npm outdated` to CI. Use `dependabot` for automated updates.
- [ ] **Settings persistence:** Create `SettingsPanel.tsx` with toggles for: music volume, sfx volume, fullscreen, particle density, vsync. Persist to `localStorage` as JSON.
- [ ] **Connection quality monitor:** Display ping/latency indicator in HUD corner. Use color coding: green (<50ms), yellow (50-150ms), red (>150ms). Show packet loss percentage on hover.
- [ ] **Offline action queue:** When WebSocket disconnects, queue player inputs locally. On reconnect, replay queued actions with timestamps. Discard queue if >5 seconds stale.

### 1.6 The Armory — Testing Suite Setup

- [ ] **Test directory structure:** Create `client/src/__tests__/` for unit tests, `client/tests/e2e/` for Playwright, `server/src/tests/` for Rust tests.
- [ ] **Vitest configuration:** Set up `vitest.config.ts` with: `environment: 'jsdom'`, `globals: true`, `coverage: { provider: 'v8', reporter: ['text', 'html'] }`.
- [ ] **MSW setup:** Install `msw` and create `src/mocks/handlers.ts`. Mock all SpacetimeDB SDK calls: `db.call`, `db.query`, `subscription`. Enable via `setupWorker()` in test setup.
- [ ] **Reducer integration tests:** Create Rust tests in `server/src/lib.rs` using `#[test]` macros. Test: `register_creates_knight`, `move_player_validates_bounds`, `attack_calculates_damage`.
- [ ] **Hook testing utilities:** Build `renderWithSpacetime()` test helper that provides mock DB context. Use in `src/__tests__/useKnightPulse.spec.tsx`.
- [ ] **Test data factories:** Create `TestDataFactory.ts` for generating: valid knights, items, map tiles. Use faker.js for random but valid data.
- [ ] **CI test pipeline:** Add `npm run test:ci` that runs tests with `--coverage --run`. Upload coverage to Codecov/coveralls. Block PRs with < 80% coverage.
- [ ] **E2E test config:** Set up `playwright.config.ts` with: `baseURL`, `viewport`, `trace: 'on-first-retry'`. Configure `webServer` to start local SpacetimeDB.
- [ ] **Test isolation:** Ensure each test cleans up its data. Use `afterEach` hooks to delete test knights/items. Parallelize tests where possible.

### 1.3 Excalibur Engine — PixiJS v8

- [ ] **`GameView.tsx`:** Mount PixiJS `Application` into a `<div ref={containerRef}>`. Use `app.init({ resizeTo: window, antialias: false, resolution: window.devicePixelRatio })`. Call `app.destroy()` on unmount.
- [ ] **`game-loop.ts`:** Hook into `app.ticker.add(delta => { update(delta); })`. Keep update logic separate from PixiJS internals — pass `delta` in seconds not frames (`delta / 60`).
- [ ] **`AssetManifest.ts`:** Define bundle for PIXI `Assets.addBundle('world', { terrain: '/assets/tilesets/terrain.png', knight: '/assets/characters/knight.png' })`. Call `Assets.loadBundle('world')` once on boot.
- [ ] **`SpritePool.ts`:** Implement an object pool for `Sprite` instances to avoid GC pressure during entity spawning. Pre-allocate 64 knight sprites on load.
- [ ] **`camera.ts`:** Track player world position. Offset the root `Container` by `(-playerX + screenW/2, -playerY + screenH/2)`. Clamp to world bounds.
- [ ] **First render test:** A single hardcoded knight sprite centered on the canvas, position driven by `useKnightPulse` hook via a shared Zustand store.
- [ ] **Error boundary:** Wrap PixiJS canvas in a React Error Boundary. On render failure, display "Reconnecting..." overlay and attempt re-initialization.

### 1.4 The Forge — Content Pipeline

- [ ] **Aseprite CLI export script (`scripts/export-sprites.sh`):** `aseprite -b src/knight.ase --sheet public/assets/characters/knight.png --sheet-type columns --data public/assets/characters/knight.json`. Run via `npm run sprites`.
- [ ] **`TextureAtlas.ts`:** Parse Aseprite JSON format. Map frame names (e.g., `"knight-walk-0"`) to `PIXI.Texture` using `Texture.from(atlas, frame.rect)`. Used by `animation.ts`.
- [ ] **`scripts/tiled-export.ts`:** Node script that reads a `.tmx` Tiled file, extracts tile data layer, and `POST`s to a local SpacetimeDB reducer (`batch_load_tiles`) for bulk `MapTile` row insertion.
- [ ] **`data/items.json` schema:** `{ id: number, name: string, sprite_id: string, stat_block: { atk: number, def: number }, slot: "weapon" | "armor" | "ring" }`. Loaded by a one-time seeder reducer on server init.
- [ ] **Asset validation CI:** Add GitHub Actions step to verify all sprites in `/public/assets/` load without errors using a headless Puppeteer test.

### 1.5 The Guide — Tutorial & New Player Experience

- [ ] **Onboarding quest chain:** Auto-assign `TutorialQuest` table entries on first login. Steps: 1) Movement tutorial (reach waypoint), 2) Combat tutorial (defeat training dummy), 3) Inventory tutorial (equip starter gear), 4) Chat tutorial (say "hello").
- [ ] **`TutorialOverlay.tsx`:** Modal dialog that pauses game input. Shows animated arrow pointing to relevant UI element. Dismisses on completion.
- [ ] **Starter zone:** Designated spawn area (chunk 0,0) with no PVP, simplified enemies, and clear waypoints. Only accessible to knights level < 5.
- [ ] **Starter gear:** Auto-equip `Wooden Sword` (+2 ATK) and `Cloth Tunic` (+1 DEF) on first spawn via `register` reducer.
- [ ] **First-login bonus:** Insert `StarterPack` table row with `item_def_ids: [100, 101, 102]`. Claimable via `claim_starter_pack` reducer within 1 hour of first login.
- [ ] **Asset validation CI:** Add GitHub Actions step to verify all sprites in `/public/assets/` load without errors using a headless Puppeteer test.

---

## ⚔️ PHASE 2: Chivalry and Chaos
*Goal: Real-time multiplayer movement on a tile map with collision.*

### 2.1 Knight's Pulse — Movement & Prediction

- [ ] **`move_player(dir: Direction, seq: u32)` reducer:** `Direction` is a Rust enum `{ North, South, East, West, NE, NW, SE, SW }`. Validate target tile is walkable using `collision.rs`. Update `Knight.x/y` and `last_input_seq`.
- [ ] **`collision.rs` helper:** `fn is_walkable(x: f32, y: f32, tables: &ReducerContext) -> bool`. Queries `MapTile` table for the target tile. Returns `false` for walls, water, objects.
- [ ] **Client-side prediction (`prediction.ts`):** On input, immediately move the local knight sprite without waiting for server ack. Store predicted position keyed by `seq`. On server update, compare server `x/y` with predicted — if delta > 1px, snap-lerp to correct (`reconcile`).
- [ ] **`interpolation.ts`:** For remote knights (not the local player), lerp their rendered position from `lastServerPos` to `currentServerPos` over the tick interval (`1000ms / TICK_RATE`). Use `PIXI.Ticker` delta for sub-frame accuracy.
- [ ] **Input buffering (`input.ts`):** Queue up to 8 unacknowledged inputs. On `move_player` ack, dequeue. If queue exceeds 8, pause local prediction (server is lagging).
- [ ] **Dead reckoning for remote players:** If no update received for a remote knight in >200ms, continue moving them at their last known velocity vector until corrected.

### 2.2 The Great Map — Chunk-Based World

- [ ] **`MapTile` table:** `#[table(name = map_tile, public)]`. Fields: `chunk_x: i32`, `chunk_y: i32`, `local_x: u8`, `local_y: u8`, `tile_type: TileType`, `walkable: bool`, `layer: u8`. Composite primary key: `(chunk_x, chunk_y, local_x, local_y, layer)`.
- [ ] **`TileType` enum (Rust):** `#[derive(SpacetimeType)] enum TileType { Grass, Water, Stone, Sand, Forest, Road }`. Auto-generated to TS as a union type.
- [ ] **`load_chunk(chunk_x: i32, chunk_y: i32)` reducer:** Called by client on chunk entry. If chunk has no rows, trigger procedural generation (Phase 6). Returns subscription to that chunk's tiles.
- [ ] **`tilemap.ts` (client):** On chunk subscription update, batch-insert chunk tile data into a `PIXI.RenderTexture` using a pre-built `TileSpriteRenderer`. Only re-render dirty chunks using a dirty-flag bitset.
- [ ] **Chunk subscription management (`subscriptions.ts`):** Maintain a 3×3 chunk subscription grid around the player. On chunk boundary crossing, add new edge subscriptions and `unsubscribe()` the trailing ones.

### 2.3 Heraldry HUD — Basic UI

- [ ] **`ChatMessage` table:** `sender_identity: Identity`, `channel: ChatChannel` (`{ Global, Local, Whisper(Identity) }`), `content: String` (max 256 chars), `timestamp: Timestamp`.
- [ ] **`send_message(channel: ChatChannel, content: String)` reducer:** Rate-limit: max 3 messages per 2 seconds per identity (use rolling window in a `RateLimit` table). Strip control characters before insert.
- [ ] **`ChatBox.tsx`:** Subscribe to `ChatMessage` table for `Global` and `Local` (proximity: same chunk as player). Use `useVirtualizer` (TanStack Virtual) for the message list to handle thousands of messages without DOM bloat.
- [ ] **`Nameplate.ts` (PixiJS):** A `Container` with a `Text` child using `BitmapFont` (pre-rendered for performance). Position above knight sprite using `knight.y - NAMEPLATE_OFFSET`. Culled when off-camera.
- [ ] **`Minimap.tsx`:** `<canvas>` element separate from the game canvas. Redraws every 500ms. Plots knight positions as colored dots. Reads from the `useNearbyKnights` hook.

---

## 🏰 PHASE 3: Realm Expansion
*Goal: Combat, inventory, loot, and a deployable desktop build.*

### 3.1 Martial Law — Combat System

- [ ] **`attack(target_id: Identity)` reducer:** Range check (Chebyshev distance ≤ `MELEE_RANGE = 1.5` tiles). Compute `damage = max(1, attacker.atk - target.def + rand(-2, 2))`. Clamp `target.hp` to `[0, max_hp]`. If `target.hp == 0`, trigger `on_death`.
- [ ] **`on_death` internal fn:** Rewards XP to attacker, drops items to world (insert `WorldItem` table rows), marks knight state as `Dead`. `Dead` knights skip movement reducers.
- [ ] **`respawn(respawn_point: Vec2)` reducer:** Callable after a 5s cooldown (`Timestamp` check). Resets `hp` to `max_hp`, teleports to respawn point, clears `Dead` status.
- [ ] **`stat_block` in `Knight` table:** Add `atk: u32`, `def: u32`, `spd: f32`, `xp: u64`, `xp_to_next: u64`. `level_up` is triggered automatically inside `gain_xp` if threshold crossed.
- [ ] **`CombatEvent` table:** Ephemeral table (no persistence needed) for broadcasting `{ attacker, defender, damage, is_crit, timestamp }` to nearby clients for VFX and combat log. Subscribe client-side for 3-second window.
- [ ] **Particle system (`particles.ts`):** On `CombatEvent` subscription callback, emit a burst from `PIXI.ParticleContainer` at the defender's position. Pool and recycle `Particle` objects. Max 512 active particles.
- [ ] **Floating damage numbers (`DamageLabel.ts`):** `PIXI.Text` using `BitmapFont`. Animate upward `y -= delta * 40` and fade `alpha -= delta * 1.2`. Return to pool at `alpha <= 0`. Colour-coded: white = normal, yellow = crit, red = high damage.

### 3.2 Spoils of War — Items & Inventory

- [ ] **`Item` definition table (`item_def`):** Static, server-seeded. Fields: `id: u32`, `name: String`, `sprite_id: String`, `item_type: ItemType`, `stat_bonus: StatBlock`, `rarity: Rarity`.
- [ ] **`Inventory` table:** `(knight_id: Identity, slot_index: u8)` → `item_def_id: u32`, `quantity: u32`, `equipped: bool`. Primary key: `(knight_id, slot_index)`. 40-slot inventory.
- [ ] **`WorldItem` table:** Items lying on the ground. Fields: `id: u64`, `item_def_id: u32`, `x: f32`, `y: f32`, `chunk_x: i32`, `chunk_y: i32`, `quantity: u32`. Despawn after 5 minutes (checked in a scheduled reducer).
- [ ] **`pick_up_item(world_item_id: u64)` reducer:** Proximity check (≤ 2 tiles). Find first empty slot in `Inventory`. Insert row. Delete `WorldItem` row. Fail gracefully if inventory full (`ReducerError::InventoryFull`).
- [ ] **`equip_item(slot_index: u8)` reducer:** Set `equipped = true` for target slot, `equipped = false` for all other slots of same `item_type`. Recompute effective `atk`/`def` on `Knight` row.
- [ ] **`InventoryPanel.tsx`:** 8×5 grid of `ItemSlot` components. Drag-and-drop via `@dnd-kit/core`. Right-click context menu (equip/drop/inspect). Tooltip via `Tooltip.tsx` on hover showing item stats.

### 3.3 Fortress Containers — Native Wrappers

- [ ] **Tauri v2 setup:** Add `desktop/` dir. Run `cargo tauri init` inside it. Configure `tauri.conf.json`: `devUrl: "http://localhost:5173"`, `frontendDist: "../client/dist"`, `identifier: "com.knightland.game"`.
- [ ] **Window config:** `"width": 1280, "height": 720, "minWidth": 800, "minHeight": 600, "decorations": true, "resizable": true`. Add 4 icon sizes (`32x32`, `128x128`, `256x256`, `512x512`) in `icons/`.
- [ ] **Tauri Rust plugin for local saves:** Use `tauri-plugin-fs` to expose `save_screenshot`, `write_config_json`, and `read_config_json` commands. Map to `invoke('save_screenshot')` calls in TS.
- [ ] **Build pipeline:** `npm run tauri build` produces `.msi` (Windows) and `.dmg` (macOS) from a single command via `tauri.conf.json` `bundle` section.
- [ ] **PWA manifest (`public/manifest.json`):** `"name": "Knightland"`, `"display": "fullscreen"`, `"orientation": "landscape"`, `"start_url": "/"`. Register a Service Worker (`sw.ts` via Vite PWA plugin) to cache `/assets/` bundles offline.
- [ ] **`vite-plugin-pwa` config:** Cache strategy: `CacheFirst` for all `/assets/`, `NetworkFirst` for the root HTML. Precache the terrain/character spritesheets (biggest assets) at install time.

### 3.4 The Oracle — Observability & Tracing

- [ ] **OpenTelemetry in Rust:** Add `opentelemetry`, `opentelemetry_sdk`, `opentelemetry-otlp` crates. Initialize a `TracerProvider` in `lib.rs` pointing to either local OTLP collector or Arize Phoenix endpoint.
- [ ] **Span each reducer:** Wrap reducer bodies in `tracer.start_active_span("move_player")`. Attach attributes: `knight.identity`, `delta_x`, `delta_y`, `input_seq`. Measure reducer execution time as a histogram.
- [ ] **SpacetimeDB module metrics:** Track `player.count.active` (gauge), `reducer.error.count` (counter), `chunk.load.duration_ms` (histogram) via OpenTelemetry Metrics API.
- [ ] **Client instrumentation (`telemetry.ts`):** Use `@opentelemetry/sdk-web`. Measure `ws.latency` (ping round-trip per `heartbeat` reducer call), `fps.render` (from PixiJS `ticker.FPS`), `chunk.subscription.count`.
- [ ] **Arize Phoenix dashboard:** Configure to ingest OTLP traces. Set up alerts for: reducer `p99 > 50ms`, `ws.latency > 200ms`, `player.count.active > 500` (scale warning).

### 3.5 The Bard's Tale — Audio

- [ ] **Install `howler.js@^2.2`** and `@types/howler`.
- [ ] **`AudioManager.ts`:** Singleton class. `load(key, src)`, `play(key, opts?)`, `stop(key)`, `setMasterVolume(v: number)`, `setMusicVolume(v: number)`, `setSfxVolume(v: number)`. Persist volumes to `localStorage`.
- [ ] **BGM system:** Crossfade between tracks using two `Howl` instances (`Howl.fade()`). Zone-based BGM: subscribe to current `chunk_biome` field in `MapTile` aggregate, change track on biome change.
- [ ] **Positional SFX:** Use Howler's `pos()` and `pannerAttr()` to set 3D audio position relative to camera center for footsteps, combat hits, and spell casts. Attenuate linearly from `refDistance: 2 tiles` to `maxDistance: 10 tiles`.
- [ ] **SFX event bus:** Subscribe to `CombatEvent` table; on row insert call `AudioManager.play('sword_hit', { pos: [event.x, event.y] })`. Same for `ItemPickup`, `LevelUp`, `PlayerDeath`.

### 3.6 The Wayfarer — Mobile & Touch Controls

- [ ] **Virtual joystick (`VirtualJoystick.tsx`):** 120px diameter, semi-transparent, bottom-left corner. Touch-drag returns direction vector. Dead zone: 10px radius.
- [ ] **Action buttons:** Context-sensitive attack/interact button (bottom-right). Size: 64px. Shows icon of current target action.
- [ ] **Touch gesture support:** Two-finger tap to open inventory. Pinch disabled (fixed zoom). Swipe right on message to quick-reply.
- [ ] **Responsive HUD layout:** Use CSS Grid with `minmax()` for panels. Collapse chat on screens < 768px width.
- [ ] **Device performance detection:** On startup, measure `requestAnimationFrame` consistency. Auto-downscale renderer to 0.5x on low-end devices (detected by average FPS < 30 in first 5 seconds).

### 3.7 The Sage — Accessibility & Internationalization

- [ ] **i18n setup:** Install `i18next` and `react-i18next`. Create `locales/en.json`, `locales/es.json`, `locales/fr.json`, `locales/de.json`. Lazy-load locale based on browser `navigator.language`.
- [ ] **Text-to-speech mode:** Add `accessibility.preferences.ttsEnabled` to localStorage. When enabled, read aloud NPC dialogue, quest objectives, and item tooltips via Web Speech API.
- [ ] **High contrast mode:** Add CSS class `.high-contrast` to root. Overrides sprite colors with outline-only rendering, increases UI contrast ratios to 7:1.
- [ ] **Screen reader support:** Add `aria-label` to all HUD elements. Use `role="button"` for interactables. Announce combat events via `aria-live="polite"`.
- [ ] **Configurable keybindings:** Store keybindings in `localStorage` as JSON. `InputManager.ts` reads from config. Default bindings: WASD movement, Space attack, E interact, I inventory, Esc menu.
- [ ] **Colorblind modes:** Add option for `deuteranopia`, `protanopia`, `tritanopia` filters via PixiJS `ColorMatrixFilter`. Recolors health bars, damage numbers, and map markers.

### 3.8 The Chronicler — UI Polish & Notifications

- [ ] **Loading screen system:** Implement `LoadingScreen.tsx` with progress bar. Show asset loading percentages during chunk loads and initial connect. Use `requestAnimationFrame` for smooth progress updates.
- [ ] **Toast notification system:** Create `ToastContainer.tsx` with queue. Support types: info, success, warning, error. Auto-dismiss after 4 seconds. Stack vertically from bottom-right.
- [ ] **Transition animations:** Add fade-in/out between screens (login → game). Use Framer Motion for UI transitions. Respect `reduced-motion` preference.
- [ ] **Character appearance customization:** `KnightAppearance` table: `identity, hair_color, skin_tone, armor_style`. Add `edit_appearance` reducer with in-game mirror UI. First edit free, premium currency after.
- [ ] **Achievement system:** `AchievementDef` table: `id, title, description, icon_sprite, xp_reward`. `KnightAchievement` table: track unlocked_at. Display in `/achievements` panel with progress bars.

### 3.9 The Warden — Player Support Systems

- [ ] **Help/FAQ system:** `HelpArticle` table: `id, category, question, answer, order`. Searchable in-game via `/help` command or menu. Categories: Controls, Combat, Economy, Account.
- [ ] **Player report system:** `PlayerReport` table: `reporter_id, reported_id, reason: ReportReason { Spam, Harassment, Cheating, Other }, evidence, timestamp`. Queue for moderator review.
- [ ] **GM tools:** Add `gm_teleport(knight_id, x, y)`, `gm_mute(knight_id, duration_minutes)`, `gm_freeze(knight_id)` reducers. Accessible only via `/admin` route with `gm` flag in Knight table.
- [ ] **Account deletion:** `delete_account` reducer with 7-day cooldown. Flag account as `pending_deletion`, then purge all personal data after period. Send confirmation emails.
- [ ] **Data export (GDPR):** `request_data_export` reducer. Generate JSON containing: inventory, achievements, chat history. Email download link within 24 hours.

---

## 🌌 PHASE 4: The Golden Age
*Goal: MMO-scale social and economy systems.*

### 4.1 Quests & Guilds

- [ ] **`Quest` definition table:** `id: u32`, `title: String`, `objective_type: ObjectiveType` (`{ KillNPC(npc_type, count), CollectItem(item_def_id, count), ReachLocation(x, y, radius) }`), `reward_xp: u64`, `reward_items: Vec<u32>`.
- [ ] **`KnightQuest` tracking table:** `(knight_id, quest_id)` → `progress: u32`, `state: QuestState` (`{ Active, Complete, Turned_In }`). Updated inside relevant reducers (e.g., `on_death` increments kill quests).
- [ ] **`Guild` table:** `id: u64`, `name: String` (unique), `leader_id: Identity`, `member_count: u32`, `description: String`. Max 50 members enforced in reducer.
- [ ] **`GuildMember` table:** `(guild_id, knight_id)` → `rank: GuildRank` (`{ Leader, Officer, Member }`), `joined_at: Timestamp`.
- [ ] **`GuildChat` channel:** Extend `ChatChannel` enum with `Guild(guild_id: u64)`. Subscriber filter: only knights whose `GuildMember` row matches.

### 4.2 The Marketplace — Economy

- [ ] **`SaleOrder` table:** `id: u64`, `seller_id: Identity`, `item_def_id: u32`, `quantity: u32`, `price_per_unit: u64` (gold coins), `listed_at: Timestamp`.
- [ ] **`list_item(slot_index: u8, quantity: u32, price: u64)` reducer:** Escrow item from Inventory (remove row), insert `SaleOrder`. Max 10 active listings per knight.
- [ ] **`buy_item(order_id: u64, quantity: u32)` reducer:** Atomic transaction: deduct gold from buyer `Knight.gold`, add to seller `Knight.gold`, transfer item to buyer Inventory, update/delete `SaleOrder`. Deduct 5% marketplace tax (sent to a global `Treasury` row).
- [ ] **`AuctionBid` table:** `order_id: u64`, `bidder_id: Identity`, `amount: u64`, `placed_at: Timestamp`. Sorted by `amount DESC`. Winner on auction close gets item.
- [ ] **`Marketplace.tsx`:** Paginated listing (25/page). Filter by `item_type`, `rarity`, sort by `price ASC/DESC`. SpacetimeDB subscription with SQL filters: `SELECT * FROM sale_order WHERE item_def_id = ? ORDER BY price_per_unit ASC LIMIT 25 OFFSET ?`.

### 4.3 NPCs, Skills & Abilities

- [ ] **`NPC` definition table:** `id: u32`, `name: String`, `sprite_id: String`, `npc_type: NPCType` (`{ QuestGiver, Shopkeeper, Trainer, Banker }`), `spawn_chunks: Vec<(i32, i32)>`, `dialogue_tree: DialogueNode`.
- [ ] **`DialogueTree` system:** JSON-defined dialogue with branches. Support: text display, choices, conditionals (quest state, item ownership), rewards (items, xp, quest_start/finish).
- [ ] **`Skill` definition table:** `id: u32`, `name: String`, `description: String`, `cooldown_secs: u32`, `mana_cost: u32`, `icon_sprite: String`.
- [ ] **`KnightSkill` table:** `(knight_id, skill_id)` → `unlocked_at: Timestamp`, `level: u32`. Skills gain XP through use, auto-level every 1000 XP.
- [ ] **`use_skill(skill_id, target_id)` reducer:** Validate cooldown (check `last_skill_use` in Knight table), deduct mana, apply skill effect. Effects: damage, heal, buff, debuff.
- [ ] **`Buff` / `Debuff` system:** `ActiveEffect` table: `knight_id, effect_type, source_id, started_at, duration_secs, stat_modifiers: JSON`. Apply stat modifiers in `calculate_combat_stats()`.
- [ ] **`Companion` / Pet system:** `Pet` table: `id: u64`, `owner_id: Identity`, `pet_def_id: u32`, `name: String`, `level: u32`, `hunger: u32`. Follows owner, provides stat bonuses. Feed via `feed_pet` reducer.

### 4.4 World Sharding

- [ ] **Horizontal zone partitioning:** Divide the world map into `Zone` regions. Each zone runs on a separate SpacetimeDB module. Portal reducers handle cross-zone teleportation by deleting the knight from Zone A's module and inserting into Zone B's.
- [ ] **`ZonePortal` table:** `from_zone: u32`, `to_zone: u32`, `entry_x/y: f32`, `exit_x/y: f32`. Client seamlessly reconnects WebSocket to the new zone's module endpoint on portal activation.
- [ ] **Global services module:** A separate SpacetimeDB module for cross-zone data: `Leaderboard`, `GuildRegistry`, `GlobalChat`, `Marketplace`. All zones subscribe to it.
- [ ] **Concurrency limits per zone:** Enforce `MAX_KNIGHTS_PER_ZONE = 200` in `login` reducer. Return error and suggest alternate zone if full.

### 4.4 Player Housing

- [ ] **`PlayerRealm` table:** `knight_id: Identity`, `realm_id: u64` (UUID), `module_name: String` (the dynamically provisioned SpacetimeDB module). Created on first home purchase.
- [ ] **Instanced zone provisioning:** Call SpacetimeDB Cloud API to create a new module instance per new realm. Store `module_name` in `PlayerRealm`. Client connects to it on `/realm` route.
- [ ] **`PlacedObject` table (per realm module):** `(realm_id, object_id)` → `item_def_id: u32`, `x: u16`, `y: u16`, `rotation: u8`. Housing items are a subcategory of `ItemType::Furniture`.
- [ ] **`place_object` / `remove_object` reducers:** Validate placement rules (no overlap, within bounds), update `PlacedObject` table. Changes broadcast instantly to all visitors in the instance.

---

## 🌩️ PHASE 5: The Eternal Realm — DevOps & Live-Ops
*Goal: Automated quality gates, zero-downtime deploys, and scalable infrastructure.*

- [ ] **GitHub Actions — Rust CI:** On `push` to `server/`, run `cargo test`, `cargo clippy -- -D warnings`, `cargo audit`. Build the WASM module (`spacetime build`) and verify it compiles cleanly.
- [ ] **GitHub Actions — Client CI:** On `push` to `client/`, run `npm run type-check` (tsc --noEmit), `npm run lint` (ESLint with `typescript-eslint`), `npm run test` (Vitest unit tests), `npm run build`.
- [ ] **Performance benchmarking:** Add `criterion.rs` benchmarks for server-side systems. Create `benches/` directory for critical path measurements (movement, combat, inventory ops).
- [ ] **Tauri Release Pipeline:** On `git tag v*.*.*`, trigger a matrix build (`ubuntu-latest`, `windows-latest`, `macos-latest`). Upload `.AppImage`, `.msi`, `.dmg` as GitHub Release assets.
- [ ] **`spacetime publish --skip-if-unchanged`:** Hash the compiled WASM module. Only publish to SpacetimeDB Cloud if the hash differs from the live deployment. Avoids unnecessary client reconnects.
- [ ] **SpacetimeDB migration strategy:** Use `#[reducer(init)]` for seeding and `#[reducer(update)]` for schema migrations. Maintain a `schema_version: u32` row in a `Metadata` singleton table. Reducers guard against running on wrong schema version.
- [ ] **Capacitor OTA:** Configure `@capacitor/app-update` to check a `/version.json` endpoint on app launch. If version differs, prompt user to fetch the latest bundle from Cloudflare R2 and hot-reload the WebView.
- [ ] **Cloudflare R2 asset CDN:** Upload all sprite sheets, audio, and fonts to an R2 bucket. Serve via `assets.knightland.gg`. Configure 1-year cache headers with content-hash filenames for aggressive caching.
- [ ] **Load testing with k6:** Write a `k6` script that simulates 500 concurrent knights connecting, authenticating, and sending movement reducers at 10Hz. Target: p99 reducer latency < 30ms under load.
- [ ] **Server performance monitoring:** Add OpenTelemetry metrics for reducer execution time histograms, connection pool usage, and memory allocation tracking.
- [ ] **Client performance budgets:** Set Lighthouse performance targets: FCP < 1.5s, TTI < 3s on mid-tier devices. Integrate into CI with `lighthouse-ci`.
- [ ] **Bundle optimization:** Use `rollup-plugin-visualizer` to analyze bundle size. Implement code-splitting for admin panel and tutorial systems.
- [ ] **Database optimization:** Add indexes on frequently queried columns (`chunk_x`, `chunk_y`, `identity`). Monitor query execution plans with `EXPLAIN ANALYZE`.
- [ ] **Version compatibility matrix:** Client sends `client_version` on connect. Server rejects if version < `min_supported_version`. Show "Update Required" screen if incompatible.
- [ ] **Feature flags system:** `FeatureFlag` table: `key, enabled_for: Vec<Identity>, percentage_enabled: f32`. Toggle features without redeploying. Use in reducers: `if is_feature_enabled("new_combat") { ... }`.
- [ ] **Gradual rollout:** When releasing major features, enable for 5% → 25% → 50% → 100% of players over 7 days. Monitor error rates at each stage.
- [ ] **Analytics pipeline:** Add anonymous event tracking (`event: string, properties: JSON, timestamp`). Track: tutorial_completion_rate, first_item_purchased, average_session_length. Use Mixpanel or self-hosted PostHog.
- [ ] **Retention funnel tracking:** Track cohort retention (Day 1, Day 7, Day 30). Record when players reach level 5, 10, 20. Identify drop-off points for optimization.

### 5.1 The Shield — Security & Anti-Cheat

- [ ] **Server-authoritative validation:** ALL game logic must run in reducers. Client predictions are visual only — server recomputes every action (movement, combat, inventory) and rejects invalid states.
- [ ] **Rate limiting per identity:** Implement `RateLimit` table with sliding window. Block reducers if > 50 actions/second from single identity. Log violations to `CheatLog` table.
- [ ] **Input sanitization:** In every reducer, validate all numeric arguments (`x`, `y`, `damage`) are within expected bounds. Reject if `x` or `y` differs from client-reported by > 2 tiles (teleport detection).
- [ ] **Client-side tamper detection:** Hash game state on client. On reducer call, send hash alongside payload. Server flags if hash mismatch indicates modified client memory.
- [ ] **WASM module integrity:** Sign the compiled SpacetimeDB WASM module. Clients verify signature before connecting. Prevents server-side code injection.
- [ ] **DDOS protection:** Configure Cloudflare rate limiting on the WebSocket endpoint. Block IPs exceeding 100 connections per minute.
- [ ] **Secure authentication:** Implement OAuth2/OIDC for external auth providers. Store only auth tokens (never passwords) in localStorage with HttpOnly flags where possible.
- [ ] **Dependency scanning:** Add `cargo audit` and `npm audit` to CI pipeline. Use `cargo-deny` to enforce license and vulnerability policies.
- [ ] **SQL injection prevention:** Use SpacetimeDB's parameterized queries exclusively. Never concatenate user input into SQL strings.
- [ ] **Secrets management:** Store API keys in GitHub Secrets. Use `tauri-plugin-secure-storage` for local secrets in desktop clients.
- [ ] **Regular security audits:** Schedule quarterly third-party penetration testing. Maintain a `SECURITY.md` file with responsible disclosure process.

### 5.2 Security & Compliance

- [ ] **Threat model refinement:** Document trust boundaries for browser, desktop wrapper, SpacetimeDB module, admin tools, CDN. Map abuse cases for cheating, spam, impersonation, leaked secrets.
- [ ] **Admin audit trails:** Emit `AdminAuditLog` for every privileged action (Quest/Item/WorldEvent mutations, zone changes). Include: timestamp, identity, payload, action type.
- [ ] **Secrets rotation cadence:** Schedule API key, TLS cert, and token rotation. Automate rotation where possible. Document rotation policy in `SECURITY.md`.
- [ ] **Environment config validation:** Add `.env.example` with strict boot-time validation. Fail fast if required values (module name, host URL, telemetry endpoint) are missing.
- [ ] **HttpOnly + secure cookies:** For external auth, prefer HttpOnly cookies. Prohibit session secrets in localStorage. Use platform secure storage.
- [ ] **Secret scanning in CI:** Integrate `gitleaks` or similar to pre-commit and CI checks. Block leaked keys, tokens, `.env` commits.
- [ ] **Disaster recovery drills:** Schedule quarterly restore drills with documented runbooks. Test rollback procedures before live tools ship.
- [ ] **Security policy documentation:** Maintain `SECURITY.md` with incident response playbooks, contact points, escalation paths, responsible disclosure process.
- [ ] **TLS/SSL enforcement:** Enforce HTTPS for all connections. Use modern TLS 1.3. Configure HSTS headers.
- [ ] **IP allowlisting for admin:** Restrict `/admin` routes to known IP ranges. Add IP-based access control for sensitive reducers.

### 5.3 Observability & Reliability

- [ ] **Correlation IDs:** Propagate unique `request_id` across client → server reducers → DB calls. Include in all logs for end-to-end tracing.
- [ ] **End-to-end tracing:** Ensure OpenTelemetry traces span client action → reducer → DB commit. Add trace-based latency budgets per operation.
- [ ] **SLOs/SLIs:** Define service level objectives: p99 reducer latency < 50ms (baseline load), 99.9% uptime target. Track in dashboards with error budgets.
- [ ] **Grafana dashboards:** Create dashboards for latency (p50/p95/p99), error rate, active players, memory usage, DB size. Link to alert rules.
- [ ] **Chaos testing plan:** Script fault injections (latency spikes, WebSocket drops, partial outages) in staging. Measure recovery time and alerting effectiveness.
- [ ] **Structured logging:** Standardize log format across server, client, desktop wrapper. Include: timestamp, trace_id, session_id, identity, reducer_name, level.
- [ ] **Log aggregation:** Deploy Loki or ELK stack. Index by: timestamp, identity, reducer, zone. Create saved filters for common queries (player issues, error patterns).
- [ ] **Reliability soak tests:** Run 30-60 minute idle/movement sessions. Detect memory leaks, reconnect loops, stale subscriptions, object-pool churn.
- [ ] **Alert routing:** Configure PagerDuty or OpsGenie for alert routing. Define on-call rotation and escalation policies.
- [ ] **Health check endpoint:** Add `/health` reducer returning: module_version, uptime, active_connections, last_migration, error_count_last_hour.
- [ ] **Graceful degradation:** Define fallback behaviors when non-critical services fail (e.g., analytics unavailable should not block gameplay).

### 5.4 Testing & QA Enhancements

- [ ] **Contract tests:** Validate Rust reducer contracts match generated TypeScript bindings. Test API shape, payload schemas on schema changes.
- [ ] **Mutation testing (Rust):** Add `cargo-mutants` or integrate `stryker-mutator` to verify test effectiveness. Introduce bugs and confirm tests catch them.
- [ ] **Property-based testing:** Extend `proptest` for movement, combat math, inventory invariants. Generate 1000+ random valid inputs.
- [ ] **E2E testing pipeline:** Extend Playwright tests for login → world entry → movement → combat → chat → reconnect. Run in CI with trace capture.
- [ ] **Snapshot testing:** Add JSON snapshots for reducer outputs, dialogue trees, item schemas. Detect unintended state changes in PRs.
- [ ] **Test data factories:** Create robust fixtures for knights, items, tiles. Use faker.js for random but valid data.
- [ ] **CI coverage gates:** Block PRs with < 80% line coverage. Report to Codecov/coveralls. Track branch coverage.
- [ ] **Visual regression tests:** Integrate Playwright screenshot diffing. Store baselines in `__snapshots__/visual/`. Fail build on > 1% diff.
- [ ] **Test isolation:** Ensure each test cleans up data. Use `afterEach` hooks. Parallelize tests where possible.
- [ ] **Fuzz testing:** Add `cargo-fuzz` for reducers processing user input (chat messages, item names). Catch panics from malformed inputs.
- [ ] **Synthetic monitoring:** Deploy Playwright tests to production every 5 minutes. Verify: login, spawn, movement, chat. Alert on failures.

### 5.5 Release Engineering

- [ ] **Canary deployments:** Release to 5% → 25% → 50% → 100% of players over 7 days. Monitor error rates at each stage.
- [ ] **Feature flags system:** `FeatureFlag` table: `key, enabled_for: Vec<Identity>, percentage_enabled: f32`. Toggle without redeploying. Use in reducers: `if is_feature_enabled("new_combat") { }`.
- [ ] **Zero-downtime migrations:** Ensure migrations can roll back. Add canary migration plan. Validate schema versions before promoting.
- [ ] **Version compatibility checks:** Client sends `client_version` on connect. Server rejects if < `min_supported_version`. Show upgrade prompt.
- [ ] **Rollback procedures:** Document steps for: deployment rollback, data rollback, client notification. Include estimated RTO (Recovery Time Objective).
- [ ] **SBOM generation:** Generate Software Bill of Materials for each release. Include in CI pipeline. Track Rust/JS dependencies.
- [ ] **Artifact signing:** Sign desktop builds (MSI, DMG, AppImage) with code signing certificates. Verify integrity on download.
- [ ] **Patch notes automation:** Auto-generate from conventional commits (`git-chglog` or similar). Surface in-app via `PatchNote` table.
- [ ] **Release playbooks:** Document multi-platform release steps, pre-flight checks, rollback criteria. Test quarterly.
- [ ] **Deprecation policy:** Maintain `DEPRECATIONS.md`. Announce removals 2 releases in advance. Provide migration paths.

### 5.6 Data Governance & Compliance

- [ ] **Data export (GDPR):** `request_data_export` reducer. Generate JSON: inventory, achievements, chat history. Email download link within 24 hours.
- [ ] **Account deletion:** `delete_account` reducer with 7-day cooldown. Flag as `pending_deletion`, purge after period. Send confirmation emails.
- [ ] **Data retention policy:** Define retention windows for logs (90 days), telemetry (1 year), game state snapshots (30 days). Automate purging.
- [ ] **Anonymization:** Anonymize analytics data before storage. Remove PII from logs. Use pseudonymized identifiers for players.
- [ ] **Audit logging:** Log all data access events (reads/writes by admins). Store immutable audit trail for compliance.
- [ ] **Privacy policy:** Document data collection, usage, retention. Publish on website. Include in client (prompt on first launch).
- [ ] **Age verification:** If game has age-restricted content, add verification flow. Comply with COPPA/GDPR-k requirements.

### 5.7 Developer Experience & Documentation

- [ ] **SDK documentation:** Auto-generate API docs from Rust code. Host at `docs.knightland.gg`. Include examples for common reducers.
- [ ] **Contributor guidelines:** Add `CONTRIBUTING.md`. Define: PR process, code style, commit conventions, testing requirements.
- [ ] **Onboarding tutorial:** Create step-by-step guide for new developers. Cover: local setup, first build, running tests, debugging tips.
- [ ] **Internal API docs:** Document admin endpoints, modding APIs, internal protocols. Version and deprecate gracefully.
- [ ] **Runbook library:** Centralize operational runbooks in `DOCS/RUNBOOKS/`. Include: deployment, rollback, incident response, scaling.
- [ ] **Architecture decision records (ADRs):** Track major technical decisions in `DOCS/ADRs/`. Template: Context, Decision, Consequences.
- [ ] **Telemetry for devs:** Add dev-mode analytics. Track: which features are used, common errors, performance on dev machines.

---

## 🌪️ PHASE 6: The Living World
*Goal: Environmental immersion and dynamic world events.*

### 6.1 Sky-Watcher — Weather & Time

- [ ] **`WorldState` singleton table:** `day_time: f32` (0.0–24.0, server-incremented every second), `weather: WeatherType` (`{ Clear, Rain, Fog, Storm }`), `weather_intensity: f32` (0.0–1.0).
- [ ] **Scheduled `world_tick()` reducer:** Runs every real-world second via `#[reducer(scheduled_interval = "1s")]`. Advances `day_time`, probabilistically transitions weather states using a Markov chain.
- [ ] **Client `WorldState` subscription:** Subscribe to single `WorldState` row. Update PixiJS `ColorMatrixFilter` on the root stage: night darkens saturation, rain adds blue tint + desaturation.
- [ ] **Rain particle system:** On `weather == Rain`, emit top-down `PIXI.ParticleContainer` of rain streaks. Intensity controlled by `weather_intensity`. Pause rain particles when minimized (visibility API).
- [ ] **Gameplay impact:** In `move_player` reducer, apply `spd *= 0.8` when `WorldState.weather == Storm`. In `attack`, `accuracy -= 10` in `Fog` when target distance > 3 tiles.

### 6.2 Lumina — Dynamic Lighting

- [ ] **Lighting layer (`lighting.ts`):** A full-screen `PIXI.RenderTexture` filled black (`alpha = 1.0`), composited over the world via `BlendMode.MULTIPLY`. Light sources punch holes using `BlendMode.ERASE`.
- [ ] **`LightSource` PixiJS object:** `radius: number`, `color: number` (hex), `intensity: number`. Draws a radial gradient `Graphics` object into the lighting texture at world position.
- [ ] **Night-time ambient:** When `day_time < 6.0 || day_time > 20.0`, set base light layer `alpha = 0.75`. At exactly midnight, `alpha = 1.0` (pitch black — only held torches provide light).
- [ ] **`Torch` item category:** Equip in off-hand slot; emits a `LightSource` of `radius: 5 tiles`, `color: 0xFFAA44`. Reflected in the lighting render texture. Multiple torches additively illuminate.
- [ ] **Spell VFX lighting:** On `CombatEvent` where `ability == Fireball`, emit a temporary `LightSource` at impact for 500ms, `radius: 8 tiles`, `color: 0xFF4400`. Fade out via `Tween` on `intensity`.

### 6.3 The Great Convergence — Scheduled World Events

- [ ] **`WorldEvent` table:** `id: u64`, `event_type: EventType` (`{ LunarEclipse, DragonRaid, Plague, HarvestFestival }`), `starts_at: Timestamp`, `ends_at: Timestamp`, `is_active: bool`.
- [ ] **`schedule_event(event_type, starts_at, duration_secs)` reducer:** Admin-only (identity check against `AdminList` table). Inserts `WorldEvent` row. `events_tick()` scheduler marks `is_active` at start time, spawns relevant NPCs/effects.
- [ ] **`LunarEclipse` effect:** `WorldState.day_time` locks at 0.0 for event duration. `WorldState.weather` freezes to `Storm`. All `Knight` rows get `+25% magic_power`, `-10% physical_power` while event is active (checked in `attack` reducer).
- [ ] **`DragonRaid` event:** Spawns a `Boss NPC` row with `hp: 50000`, patrol path circling the capital city chunk. Requires coordinated player group kill. On death: drops a `LegendaryDrop` world item visible to all in zone.
- [ ] **Client event ticker (`EventBanner.tsx`):** Subscribes to `WorldEvent` table. Displays animated full-width banner on event start with countdown timer. Uses `Intl.RelativeTimeFormat` for localized time display.

### 6.4 Chronicles — Lore & Discovery

- [ ] **`Lore` definition table:** `id: u32`, `title: String`, `content: String` (Markdown), `trigger_type: LoreTrigger` (`{ ProximityToTile(x, y, radius), ItemPickup(item_def_id), NPCDialogue(npc_id) }`).
- [ ] **`KnightLore` tracking table:** `(knight_id, lore_id)` → `discovered_at: Timestamp`. Inserted when trigger fires in relevant reducer.
- [ ] **`Codex.tsx`:** A scrollable panel grouped by `lore_type` (`History`, `Bestiary`, `Geography`). Undiscovered entries show `???`. Progress: `X / Y discovered` per category.
- [ ] **`LoreGlyph.ts` (PixiJS):** Animated glowing sprite placed in the world at lore trigger coordinates. Pulses using `PIXI.BlurFilter` oscillation. Disappears per-instance once a knight discovers it.

---

## 📜 Roadmap Philosophy

- **Web First, Always:** Every feature is validated in the browser before being wrapped for desktop/mobile.
- **Server is Truth:** No game-affecting logic runs exclusively on the client. All state changes go through SpacetimeDB reducers.
- **Measure Everything:** The Oracle (observability) is not optional — instrument as you build, not after.
- **Schema Migrations Over Rewrites:** Use `#[reducer(update)]` and the `schema_version` guard pattern. Never delete a table; add new ones.
- **Pool, Don't Allocate:** Keep GC pressure low on the client. Object pools for particles, sprites, and labels are non-negotiable for smooth 60fps.

---

## 🔄 Live Operations — Content Pipeline

- [ ] **Admin dashboard (`/admin` route):** Protected route with `admin` flag in `Knight` table. CRUD interface for `Quest`, `Item`, `WorldEvent` tables.
- [ ] **Content bundle format:** JSON files in `/data/bundles/` for quests, items, NPCs. Loaded via `admin_reload_bundle(bundle_name)` reducer.
- [ ] **Hotfix reducer:** `update_table_entry(table, primary_key, field, new_value)` — emergency fixes without full module redeploy.
- [ ] **Rollback capability:** Maintain last 5 `schema_version` migrations as revert scripts in `/server/migrations/`.
- [ ] **Player feedback system:** `BugReport` table: `knight_id, subject, description, screenshot_url, timestamp`. Admin reviews in dashboard.
- [ ] **Patch note system:** `PatchNote` table: `version: String`, `content: String`, `posted_at: Timestamp`. Client fetches on login, displays in "What's New" modal if new version.
