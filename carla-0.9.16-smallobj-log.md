## 🧠 CARLA Development Log — October 7, 2025
**Project:** Behavior Agent + Pygame Visualization  
**Developer:** Muzaffer Citir  
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
- `docker stop $(docker ps -q)` returned “no running containers”.
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
  `-v ~/Carla_UE_docker_official/CarlaProjects:/workspaces/carla/UserContent`

✅ **Result:** Carla server launched successfully (`-vulkan -nosound`).  
⏱ **Duration:** ~40 min.

---

### 🕒 12:15 — Behavior Agent Recording Script
**Goal:** Run and stabilize Python script (`carla_record_behavior_agent.py`).

**Actions**
- Fixed `ModuleNotFoundError: No module named 'cv2'`.
- Decided to install **full OpenCV** instead of headless (for future visualization).
- Verified all imports worked correctly:  
  `BehaviorAgent`, `GlobalRoutePlanner`, `PIL`, `numpy`, `matplotlib`, etc.
- Confirmed image saving, FPS measurement, and spectator follow threads were functional.

✅ **Result:** Core recording loop operational and stable.  
⏱ **Duration:** ~1h.

---

### 🕒 13:15 — Traffic Light and Route Improvements
**Goal:** Reduce waiting times at intersections and ensure proper ego spawn orientation.

**Changes**
```python
light.set_red_time(2.0)
light.set_yellow_time(1.0)
light.set_green_time(4.0)
