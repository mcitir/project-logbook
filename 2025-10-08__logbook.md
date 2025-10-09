## 🧠 CARLA Behavior Agent Development Logbook

**Date:** 2025-10-08  
**Project:** `carla_behavior_agent_v8_scenario_profiles`  
**Developer:** Muzaffer Çıtır  
**Location:** TU Berlin – Smart Mobility Systems Group  
**Environment:** Docker (CARLA 0.9.16 custom)  
**Machine:** RTX 4090, Ubuntu 22.04 container  
**Total active work time:** ~3.25 hours

---

### 🕓 Chronological Development Timeline

#### **00:15 – 00:45 | Core Stabilization**
- Started from the previously stable `v7_flexible_ui` build.
- Verified pedestrian AI walking logic and FPS measurements.
- Observed that pedestrians were walking in place — fixed by ensuring controllers' destinations are regularly updated.
- ✅ **Result:** Pedestrians now walk smoothly across sidewalks.

---

#### **00:45 – 01:30 | Configuration Refactor**
- Extracted parameters for easier runtime tuning:
  - Traffic-light durations (`RED/YELLOW/GREEN_TIME`)
  - Pedestrian density and speed range
  - Weather presets
- Moved all constants to a centralized **CONFIG block**.
- ✅ **Result:** All environment controls can be edited without touching any logic.

---

#### **01:30 – 02:15 | Minimap Rewrite (pygame)**
- Replaced Matplotlib-based minimap with a lightweight **pygame overlay**.
- Added padding and white background for better visual clarity.
- Implemented `MINIMAP_POSITION` for adjustable screen placement.
- ✅ **Result:** Stable, non-flickering minimap window; configurable via settings.
- ⚙️ Default: `(x = 200, y = 700)` → lower-left placement.

---

#### **02:15 – 02:45 | Route Configuration Improvements**
- Integrated flexible route generation:
  - Added `ROUTE_MIN_DISTANCE` and `ROUTE_MAX_ATTEMPTS` to CONFIG.
  - Ensured route generation dynamically adapts based on scenario.
- ✅ **Result:** Reliable route generation even on complex maps (`Town10HD_Opt`).

---

#### **02:45 – 03:15 | Debug Visual Optimization**
- Tuned 3D route line rendering:
  - Reduced thickness → `0.15`
  - Changed color → darker blue `(0, 0, 100)`
  - Shortened lifetime → `0.5 s`
- Attempted to reduce glowing artifacts near lines.
- ✅ **Result:** Improved visual balance under bright weather; minor glow persists but acceptable.

---

#### **03:15 – 04:00 | Scenario Profile System**
- Introduced **SCENARIO_PROFILES**, enabling predefined environment configurations:
  - `"city_rain"` → Wet Cloudy Noon, 80 pedestrians
  - `"sunny_suburb"` → Clear Noon, higher ego speed
  - `"night_test"` → Soft Rain Sunset, moderate traffic
- Added `ACTIVE_SCENARIO` selector for quick switching.
- Implemented override:
```python
CONFIG.update(SCENARIO_PROFILES[ACTIVE_SCENARIO])
```
- ✅ **Result:** One script now simulates multiple cities and weather conditions seamlessly.

---

#### **04:00 – 04:30 | Integration Test**
- Ran full simulation cycles for all three profiles:
  - 🏙️ **city_rain:** smooth traffic flow, balanced exposure
  - 🌇 **sunny_suburb:** bright and stable visuals
  - 🌃 **night_test:** realistic lighting & wet reflections
- Verified timestamped capture folders and clean shutdown.
- ✅ **Result:** Stable execution with proper cleanup on `KeyboardInterrupt`.

---

#### **04:30 – 05:15 | Documentation & Logging**
- Verified FPS averages (≈ 17–20 FPS @ 1920×1280).
- Confirmed `enable_postprocess_effects = True` preserves realistic camera output.
- Reorganized repo layout and finalized naming:

---

### 🧩 Technical Summary

| Component | Description | Status |
|-----------|-------------|--------|
| **Scenario Profiles** | Dynamic multi-environment setup | ✅ |
| **Config System** | Centralized global dictionary | ✅ |
| **Pedestrian AI** | Controlled, continuous walking | ✅ |
| **Camera Capture** | Thread-safe JPEG writer | ✅ |
| **Debug Route Lines** | Non-persistent, visually subtle | ✅ |
| **Minimap Overlay** | Real-time pygame rendering | ✅ |
| **Weather Control** | Adjustable per scenario | ✅ |
| **Thread Cleanup** | Full resource termination | ✅ |

---

### ⏱️ Total Work Duration

| Task | Duration |
|------|----------|
| Stabilization & Refactor | 0 h 30 min |
| Visualization & Minimap | 0 h 45 min |
| Scenario Profile System | 0 h 45 min |
| Testing & Validation | 0 h 30 min |
| Documentation & Cleanup | 0 h 45 min |
| **Total Active Work** | **≈ 3 h 15 min (00:15 – 05:15 session)** |
