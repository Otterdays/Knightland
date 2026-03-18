# Knightland: Security Bill of Materials (SBOM) 🛡️
**Project:** Knightland  
**Last Updated:** 2026-03-12  
**Status:** Planning phase. All dependencies are for the **Citadel Stack**.

---

## 🏗️ Project RoundTable — Rust (Backend)

| Package (Crate) | Version | Date Added | Notes |
|---|---|---|---|
| `spacetimedb` | latest | 2026-03-12 | Core Server/DB SDK |
| `serde` | `1.x` | 2026-03-12 | Serialization |

---

## 🎨 Heraldry Interface — TypeScript (Frontend)

| Package (npm) | Version | Date Added | Notes |
|---|---|---|---|
| `vite` | `^6.x` | 2026-03-12 | Fast Dev Server |
| `react` | `^19.x` | 2026-03-12 | UI Layer |
| `spacetimedb` | latest | 2026-03-12 | Knight's Pulse (Sync) |
| `pixi.js` | `^8.x` | 2026-03-12 | Excalibur Engine (Renderer) |

---

## 🏰 Fortress Containers — Tauri (Desktop)

| Package | Version | Date Added | Notes |
|---|---|---|---|
| `tauri` | `^2.x` | (Phase 2) | Native Wrapper |

---

## 🛡️ Security Checklist
- [ ] No secrets committed.
- [ ] `npm audit` and `cargo audit` scheduled.
