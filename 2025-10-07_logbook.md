## 🧠 CARLA Development Log — October 7, 2025

**Project:** Behavior Agent + Pygame Visualization  
**Developer:** Muzaffer Çıtır  
**Environment:** Docker (CARLA 0.9.16 custom)  
**Machine:** RTX 4090, Ubuntu 22.04 container  
**Total active work time:** ~7.5 hours

---

### 🕒 09:30 — Environment Troubleshooting

**Goal:** Reactivate GPU and Vulkan support inside the CARLA Docker container.

**Actions & Issues**
- Encountered repeated `ERROR_INITIALIZATION_FAILED` from `vulkaninfo`.
- Verified host-level drivers (`nvidia-smi` OK with CUDA 12.9).
- Found missing Vulkan ICD bindings inside container.
  → Mounted host `/usr/share/vulkan/icd.d/nvidia_icd.json` manually.

**Fix**
- Exported environment variables:
  `VK_ICD_FILENAMES`, `VK_LAYER_PATH`, `__GL_ALLOW_UNSUPPORTED_PLATFORM`.
- Confirmed GPU visibility: `deviceName = NVIDIA GeForce RTX 4090`.

✅ **Result:** Vulkan working inside container.  
⏱ **Duration:** ~1h 15m.

---

### 🕒 10:45 — Container Management & Cleanup

**Goal:** Properly handle stale CARLA containers blocking new sessions.

**Actions**
- `docker stop $(docker ps -q)` returned "no running containers".
- Found persistent stopped container named `/carla_gui`.
- Used `docker rm -f carla_gui` to clean up.
- Wrote small cleanup script `reset_docker_env.sh` to:
  - Stop all running containers safely,
  - Remove only named stale ones,
  - Avoid pruning images or networks.

✅ **Result:** Container environment now cleanly reusable.  
⏱ **Duration:** ~45 min.

---

### 🕒 11:30 — Carla Server Re-Run with Vulkan

**Goal:** Run CARLA under Vulkan with full GPU acceleration.

**Actions**
- Added correct volume mounts:
  - `/usr/share/vulkan/icd.d/nvidia_icd.json`
  - `/usr/lib/x86_64-linux-gnu/libGLX_nvidia.so.*`
- Added working directory mount:
  - `v ~/Carla_UE_docker_official/CarlaProjects:/workspaces/carla/UserContent`

✅ **Result:** Carla server launched successfully (`-vulkan -nosound`).  
⏱ **Duration:** ~40 min.

---

### 🕒 12:15 — Behavior Agent Recording Script

**Goal:** Run and stabilize Python script (`carla_record_behavior_agent.py`).

**Actions**
- Fixed `ModuleNotFoundError: No module named 'cv2'`.
- Decided to install **full OpenCV** instead of headless (for later visualization).
- Verified all imports worked: `BehaviorAgent`, `GlobalRoutePlanner`, `PIL`, etc.
- Confirmed image saving, FPS measurement, and spectator follow threads functional.

✅ **Result:** Core recording loop operational.  
⏱ **Duration:** ~1h.

---

### 🕒 13:15 — Traffic Light and Route Improvements

**Goal:** Reduce waiting times at intersections and improve spawn logic.

**Changes**
- Added function to shorten traffic light times:
```python
light.set_red_time(2.0)
light.set_yellow_time(1.0)
light.set_green_time(4.0)
```
- Fixed ego spawn so that it starts exactly **at route start waypoint** with correct yaw.
- Added safety Z-offset (`+0.2`) to prevent spawn clipping.

✅ **Result:** Vehicle now starts correctly aligned with route and drives smoothly.  
⏱ **Duration:** ~1h 10m.

---

### 🕒 14:30 — Integrated 3D Route Visualization

**Goal:** Visualize blue navigation route directly in CARLA world.

**Actions**
- Used `world.debug.draw_line()` to plot 3D route dynamically.
- Verified ego (red dot) and route (blue lines) update in real time.
- Disabled old Matplotlib plotting to prevent GUI thread issues.

✅ **Result:** In-simulation route rendering stable and efficient.  
⏱ **Duration:** ~40 min.

---

### 🕒 15:15 — Pedestrian Simulation

**Goal:** Add walking pedestrians controlled by AI.

**Actions**
- Spawned pedestrians using:
```python
walker_bps = bp_lib.filter("walker.pedestrian.*")
controller_bp = bp_lib.find("controller.ai.walker")
```
- Linked AI controllers, assigned random navigation targets.
- Added configurable parameters in `CONFIG`:
  - `PEDESTRIAN_COUNT`
  - `PEDESTRIAN_SPEED_MIN/MAX`

**Issue:** Initially pedestrians "walked in place" but did not move.

**Fix:** Started controllers explicitly via `ctrl.start()` and `ctrl.go_to_location()`.

✅ **Result:** Pedestrians now walk realistically on sidewalks.  
⏱ **Duration:** ~1h 20m.

---

### 🕒 16:35 — Weather & Configuration System

**Goal:** Centralize simulation parameters.

**Actions**
- Rebuilt script header with unified `CONFIG` dictionary:
  - Weather, traffic light durations, pedestrian density, speed, and ego model.
- Added easy toggles:
  - `"SHOW_PYGAME_MAP": True`
  - `"SHOW_3D_ROUTE": True`

✅ **Result:** Simulation fully configurable from one place.  
⏱ **Duration:** ~40 min.

---

### 🕒 17:15 — Pygame Minimap Implementation

**Goal:** Replace Matplotlib with Pygame-based minimap overlay.

**Actions**
- Removed Matplotlib entirely.
- Implemented `live_minimap()` using Pygame:
  - Static blue route drawn once.
  - Red ego dot updated in real-time.
  - White background with padding for visibility.
- Added toggle flag in `CONFIG` to enable/disable map.

✅ **Result:** FPS stable (~20), no GUI thread warnings, clean overlay.  
⏱ **Duration:** ~1h 15m.

---

### 🕒 18:30 — Final Validation & Version Commit

**Goal:** Verify full integration.

**Tests**
- Spawned vehicle and pedestrians, checked weather & lights.
- Verified `SHOW_PYGAME_MAP` and `SHOW_3D_ROUTE` toggles.
- Clean shutdown without residual actors.

✅ **Result:** All components stable.  
📦 **Version saved as:** `carla_behavior_agent_v5_pygame_stable`  
⏱ **Duration:** ~45 min.

---

### 📊 Summary

| Category | Status | Time Spent |
|----------|--------|------------|
| GPU / Vulkan Fix | ✅ Success | 1h 15m |
| Docker Cleanup | ✅ Success | 45m |
| Carla Server Launch | ✅ Success | 40m |
| Recording Script Setup | ✅ Success | 1h |
| Route & Spawn Logic | ✅ Success | 1h 10m |
| 3D Visualization | ✅ Success | 40m |
| Pedestrian AI | ✅ Success | 1h 20m |
| Config System | ✅ Success | 40m |
| Pygame Minimap | ✅ Success | 1h 15m |
| Final Integration | ✅ Success | 45m |

🕐 **Total effective work time:** ≈ **7.5 hours**

---

