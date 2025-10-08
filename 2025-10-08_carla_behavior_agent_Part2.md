# 🧭 CARLA Behavior Agent Logbook – Part 2 (08 Oct 2025)

### 🕓 **Session Timeline**

| Period | Duration | Focus |
|--------|----------|-------|
| 11:19 – 11:28 | 9 min | Async vs Sync simulation discussion |
| 21:38 – 23:41 | ~2 h 03 min | Pedestrian fixes, hybrid mode reasoning, motorcycle & traffic integration, TM tuning |

### 🧩 **Main Developments**
- Reverted to the **stable v5** working codebase.
- Analyzed synchronization effects on FPS & pedestrians.
- Decided not to force pedestrians async-only mode (kept sync for realism).
- Added motorcycle spawner (`spawn_motorcycles`) and verified autonomous navigation via TM.
- Added random traffic vehicles with diverse models & colors.
- Unified cleanup section for ego + pedestrians + motorcycles + traffic.
- Maintained FPS stability (~15–20 Hz under mixed load).
- Final stable version marked as baseline.

### ✅ **Successes**
- Fully functional multi-actor environment (ego, pedestrians, motorcycles, traffic).
- Stable recording & FPS measurement system retained.
- Correct cleanup sequence on exit.

### ⚠️ **Issues / Lessons**
- TM advanced APIs (`set_boundaries`, `global_distance_to_leading_vehicle`) unsupported in 0.9.16.
- Debug-line invisibility feature not available pre-0.9.18.
- FPS briefly spiked (~80 Hz) during async test — likely camera tick mismatch.

### 🧠 **Next Steps**
- Optional: introduce lightweight route re-planner for AI vehicles.
- Optional: separate async TM for pedestrians only.
- Plan next logbook part ("Part 3") for hybrid FPS stability experiments.

### 🧾 **Summary Table**

| Module | Status | Notes |
|--------|--------|-------|
| Ego Vehicle | ✅ Stable | BehaviorAgent route switching works |
| Pedestrians | ✅ Stable | Synced AI controllers, random paths |
| Motorcycles | ✅ Added | Auto-driving via TM |
| Traffic Cars | ✅ Added | Randomized models & colors |
| FPS Monitor | ✅ Stable | Console-based |
| Minimap & Camera | ✅ Functional | Route overlay toggleable |
| TM Advanced APIs | ⚠️ Unsupported | 0.9.16 limitation |
