# 🧠 CARLA Behavior Agent Development Logbook

**Date:** 2025-10-10  
**Project:** `carla_behavior_agent_v8_scenario_profiles`  
**Developer:** Muzaffer Çıtır  
**Location:** TU Berlin – Smart Mobility Systems Group  
**Environment:** Docker (CARLA 0.9.16 custom)  
**Machine:** RTX 4090 | Ubuntu 22.04 container  
**Total active work time:** ~5 hours (13:26 – 15:46 + 23:45 – 01:23)

---

### 🕓 Chronological Development Timeline

**13:26 – 14:00 | Map & Scenario Validation**
- Attempted to start simulation using `Town07`; encountered missing-map error.
- Verified available maps via blueprint query — selected `Town03` as working fallback.
  - ✅ *Result:* Map load confirmed and simulation started correctly.

**14:00 – 14:45 | Integrated Live Terminal Logger**
- Added a persistent `log.txt` per simulation under the same level as `front` capture folder.
- Implemented custom `Tee` class to redirect both `stdout` and `stderr` to file + console simultaneously.
  - ✅ *Result:* Continuous, real-time logging established for every simulation block.

**14:45 – 15:30 | Behavior Agent Tuning (Oscillation & Throttle Stability)**
- Re-introduced controlled update intervals and throttle damping to reduce ego jitter.
- Adjusted `BehaviorAgent` PID parameters; however, v0.9.16 internal structure limited fine tuning.
  - ⚠️ *Observation:* Vehicle oscillation partially improved, but overshoot persists at turns.

**15:30 – 15:46 | Segmentation Fault Investigation (Paused)**
- Reproduced segmentation fault during multi-actor spawn (pedestrians + motorcycles + vehicles).
- Hypothesis: synchronous tick loop instability + heavy GPU scene load.
  - 🕐 *Session temporarily paused for analysis.*

**23:45 – 00:45 | Stability Rebuild**
- Consolidated all working features into a single **stable baseline script**.
- Removed experimental PID and timing modifications; restored clean tick cycle.
- Added five complete `SCENARIO_PROFILES` (`city_rain`, `sunny_suburb`, `night_test`, `dark_city`, `urban_drive`) with dynamic pedestrian/vehicle scaling and weather presets.
  - ✅ *Result:* Script executes deterministically with synchronized pedestrians, autonomous traffic, and motorcycles.

**00:45 – 01:23 | Final Integration & Cleanup Validation**
- Verified live minimap, capture writer, and per-session logger functionality.
- Ensured clean shutdown with all actors destroyed and camera thread stopped.
  - ✅ *Result:* Fully functional unified version confirmed — no runtime errors in `urban_drive` scenario.

---

### ⚙️ **Technical Summary**

| Component | Update | Status |
|-----------|--------|--------|
| **Map Loader** | Added map fallback and reload routine | ✅ Stable |
| **Logger System** | Live `log.txt` next to `front/` | ✅ Working |
| **Pedestrians** | Asynchronous controllers, variable speeds | ✅ Smooth |
| **Motorcycles & Traffic** | Autonomous via TrafficManager | ✅ Active |
| **Ego Vehicle** | BehaviorAgent (normal) + throttle cap 0.7 | ✅ Stable |
| **PID Tuning** | Removed unstable low-level access for 0.9.16 | ⚙️ Neutral |
| **3D Route / Minimap** | Optional toggles via `CONFIG` | ✅ Tested |
| **Segmentation Fault** | No occurrence after cleanup & rebuild | ✅ Resolved |

---

✅ **Final Outcome:**

All subsystems (ego + pedestrians + traffic + motorcycles) now operate in one coherent, stable loop with live logging and automatic capture directory structure. This build serves as the **new baseline version** for future scenario or perception integration work.
