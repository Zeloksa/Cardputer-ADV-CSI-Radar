![Version](https://img.shields.io/badge/Version-2.0-blue)
![Hardware](https://img.shields.io/badge/Hardware-Cardputer%20%7C%20ADV-orange)
![Platform](https://img.shields.io/badge/Platform-M5Stack-red)
![License](https://img.shields.io/badge/License-Proprietary-gray)
[![Boosty](https://img.shields.io/badge/Support-Boosty-orange)](https://boosty.to/zeloksa)

# 📡 WiFi CSI Radar (V2.0)

**Radar V2.0** is a high-performance Wi-Fi CSI (Channel State Information) sensing and security system built for the **M5Stack Cardputer** and **Cardputer ADV** (ESP32-S3). By analyzing multipath radio-wave reflections and Fresnel zone distortions across 16 to 32 OFDM subcarriers, it detects motion, breathing, and spatial presence inside rooms and through walls.

> [!IMPORTANT]
> **Source Code Status:** This project is proprietary. The source code is private.  
> **Distribution:** Binary only via **M5Burner**.

---

## ⚡ Technical Highlights

- **LAN-First Autonomous Injector (Core 0):** Generates a rock-solid **290–320 Hz PPS** packet stream even without internet access (WAN offline, isolated router, or smartphone hotspot). Uses a 2:1 LAN/WAN hybrid injection ratio (`localhost` DNS & high UDP ports `33434..33437`) with adaptive `ping_delay` (`2..10ms`) and batched lwIP socket draining.
- **24/7 Non-Blocking Wi-Fi Watchdog:** Continuously monitors uplink state in the background. If your router reboots or drops connection for >6 seconds, the radar automatically reconnects and restores CSI capture without freezing or losing your calibrated baselines.
- **Ultra-Precise 1D Sensing Core (Core 1):** Features a **Frozen Static Baseline** (so a sleeping or stationary person is never absorbed into the background) paired with a dual-loop spatial-temporal filter (fast EMA vs. 25Hz isochronous slow envelope) and a linear-quadratic energy scale.
- **NEW — Guard Security Mode (`STATE_GUARD`):** A 100% silent, stealth security logger with an 18-second exit delay, screen auto-sleep (`15s`), wake-key swallowing, NVS Flash "Black Box" event logging, `.CSV` export, **Rain Mode (`[W]`)**, and **BMI270 IMU Anti-Tamper (`[M]`)**.
- **Through-Wall vs. In-Room Classifier:** Separates neighbor activity behind walls (`WALL?`) from real room entry (`IN-ROOM`) using subcarrier spread, Doppler impulse ratios, an **AGC / Router Tx-Power Spike Rejector**, **Subcarrier Quality Weighting**, and a **Leaky Integrator** with real-time **Coherence Bar (`0–100%`)** and **Confidence Percentage (`%`)**.
- **Room Area ($m^2$) Scaling (`[A]`):** Adapts Guard Mode Fresnel thresholds to your exact room size (`AUTO` from mapped 2D polygon or manual presets from `2m²` to `45m²`), allowing you to tune out thin drywall partitions or tight spaces like bathrooms/WCs.
- **Full NVS Flash Persistence:** Room polygons (`map_nvs`), 21 spatial 2D profiles (`sp2d_nvs`), Guard security logs, and area presets (`guard_nvs`) survive power loss and reboots.
- **Zero-Heap-Fragmentation Engine:** All high-frequency Web UI telemetry (`handleData`, `handleLogs`), trig LUT wave rendering, and rotated wall labels run on static pre-allocated buffers (`snprintf`) with **0 bytes** of dynamic heap allocation.

---

## 🛠 Installation

1. Open **M5Burner**.
2. Search for `WiFi CSI Radar` or `Zeloksa`.
3. Select version **V2.0**.
4. Burn to your **M5Stack Cardputer** or **Cardputer ADV**.

---

## 🕹 Controls & Hotkeys

### 🌐 Global & Navigation Controls
| Key | Action |
| :--- | :--- |
| **`[ , ]` / `[ / ]`** | Navigate Main Menu Carousel (4 Modes) / Turn Manual Pages (`1/6..6/6`) |
| **`[ ENTER ]`** | Launch Selected Mode / Start & Stop Calibration / Hold (`1.2s`) to Reset Room Map |
| **`[ ESC / ` ]`** | Return to Main Menu (safely preserves mapped room polygon & NVS data) |
| **`[ I ]`** | Open On-Device Interactive User Manual (**6 Pages**) |
| **`[ D ]`** | Toggle Web-Terminal Server (`Port 80`) & IP Overlay |
| **`[ R ]`** | Quick-Switch between **1D Radar** and **2D Spatial Mode** |
| **`[ - ]` / `[ = ]`** | Speaker Volume Control (`0% – 100%`, default normalized to `30%`) |
| **`[ [ ]` / `[ ] ]`** | Hardware Display Brightness Control |
| **`[ DEL ]`** | Wipe saved Wi-Fi credentials & reboot (works in Main Menu & Boot Screen) |

### 🛡️ Guard Mode (`STATE_GUARD`) Exclusive Hotkeys
| Key | Action |
| :--- | :--- |
| **`[ ENTER ]`** | Arm Guard Mode (starts 18s exit delay) / Disarm |
| **`[ A ]`** | Cycle Room Area Preset: `AUTO` $\rightarrow$ `2` $\rightarrow$ `4` $\rightarrow$ `6` $\rightarrow$ `8` $\rightarrow$ `12` $\rightarrow$ `16` $\rightarrow$ `20` $\rightarrow$ `25` $\rightarrow$ `35` $\rightarrow$ `45 m²` |
| **`[ W ]`** | Toggle **Rain Mode** (filters wet windows, rain noise, and dielectric drift) |
| **`[ M ]`** | Toggle **IMU Tamper Sensor** (detects physical touch, tilt, or pickup $\rightarrow$ `TAMPER! [IMU]`) |
| **`[ F ]`** | Toggle Log Filter: `VERDICT [ALL]` vs. `VERDICT [IN]` (confirmed threats only) |
| **`[ T ]`** | Open Manual Clock Modal (`[HH:MM]`) for offline time stamping |
| **`[ C ]`** | Clear Security Event Log (wipes RAM & NVS log buffer) |
| **`[ ; ]` / `[ . ]`** | Scroll Security Event Log Up / Down |

---

## 📖 Comprehensive User Manual

### 🎯 1. 1D Radar Mode: Omnidirectional High-Sensitivity Scanner
This mode acts as an unfiltered, ultra-sensitive radio tripwire that detects motion both inside the room and through adjacent walls.
1. **Positioning:** Place the Cardputer on a stable surface. **Do not hold it in your hands.**
2. **Start Calibration (`12s`):** Press `[ENTER]`.
   - **0–3s:** Stabilization delay.
   - **3–9s:** Static multipath baseline training (`baseline_vector`).
   - **9–12s:** Noise floor profiling (`noise_ema`).
3. **The "Ghost" Rule:** Step away and remain still during the 12-second countdown.
4. **Live Detection:** Any motion triggers expanding red/yellow radar waves, classifies target size (`[S]`, `[M]`, `[L]`, `[XL]`), and plays pitch-scaled audio tones.

---

### 🛡️ 2. Guard Security Mode: Silent Stealth Logger & Wall Filter
Designed for unattended room monitoring. The onboard speaker is **100% muted** at all times so the device never gives away its position.

1. **Clock Sync:** Upon entering Guard Mode, enter the current time (`[HH:MM]`) or press `[ESC]` to skip. If you open the Web UI (`[D]`), your browser will sync the exact time (`HH:MM:SS`) automatically.
2. **Arming & Exit Delay (`18s`):** Press `[ENTER]`.
   - **18–9s (`LEAVE!`):** Walk out of the room and close the door.
   - **9–3s (`CAL`):** Records the empty room baseline.
   - **3–0s (`CAL`):** Measures subcarrier noise floor, computes the **Subcarrier Quality Mask**, and locks the 3D gravity vector if **IMU (`[M]`)** is enabled.
3. **Stealth Auto-Sleep:** After **15 seconds** of keyboard inactivity, the screen turns completely off (`0%` brightness) and pauses rendering to save battery. Pressing **any key** wakes the screen safely — the wake keypress is swallowed so you never accidentally disarm the radar in the dark.
4. **In-Room vs. Neighbor (`WALL?`) Classification:**
   - **Coherence Bar (`0–100%`):** Displayed in the Guard header (under the area indicator) and in the Web UI. Shows real-time leaky integrator energy (`evt_inroom_energy`). Neighbor movement behind a wall stays in the cyan/yellow zone (`< 50%`) and logs as `WALL? (XX%)`. Genuine room entry fills the bar to `100%` (red) and triggers `IN-ROOM (XX%)`.
5. **Event Coalescing & NVS Black Box:** Continuous motion is merged into a single session (4s cooldown) tracking start time, total duration (`s`), peak size, verdict, and confidence `%`. Up to 32 events are stored in NVS Flash (`guard_nvs`) and can be exported to `.CSV` via the Web UI.

> [!TIP]
> **Drywall, Thin Walls & Small Rooms (WC) Tuning (`[A]` Key):**  
> By default, Guard Mode uses `AUTO` area (calculated from your mapped 2D room polygon, or `16m²` if unmapped).  
> - If your room has **thin drywall partitions** and neighbor movements push the Coherence Bar too high, press **`[A]`** (or click **`AREA`** in the Web UI) and set a **smaller area (`8m²` or `12m²`)**.  
> - For **tiny spaces (bathrooms, WCs, corridors)** with thin doors, select **`2m²`, `4m²`, or `6m²`**. This tightens the Fresnel coherence window so people walking right outside the door are ignored, while anyone stepping inside is detected.

---

### 📍 3. Hybrid SLAM: Room Mapping
Uses the onboard BMI270 IMU to draw a scaled floor plan of your room and compute its exact area in **$m^2$**.
1. Select **ROOM MAPPING** from the Main Menu (or click **START ROOM MAPPING** directly inside the **2D** or **Guard Mode** Web UI).
2. **Posture:** Hold the Cardputer flat against your chest, screen facing outward.
3. **The Walk:** Press `[ENTER]` and walk along the perimeter walls at a steady pace. Step firmly on your **HEEL** (each step = `0.6m`, confirmed by a short beep).
4. **Corners:** Stop at each corner, turn 90 degrees on the spot, and continue walking.
5. **Finish:** Press `[ENTER]` (or click **STOP MAPPING** in the Web UI) when you return to the starting point. The geometry engine automatically snaps the closing corner, calculates wall lengths in meters, saves the polygon to NVS Flash, and updates the `AUTO` room area (**$m^2$**) for Guard Mode.

> [!NOTE]
> Pressing `[ESC]` to exit to the Main Menu **never** deletes your saved map. To erase the map, hold `[ENTER]` for **1.2 seconds** until the red `RESET` ring completes.

---

### 🗺️ 4. 2D Spatial Mode: Zone Profiling (Active Beta)
Tracks physical coordinates $(X, Y)$ inside your room using 32 subcarriers and 21 trained reference profiles.
1. Open **2D SPATIAL** (`[R]` or via Main Menu) and press `[D]` to launch the Web UI.
2. **Train Empty Room (Profile 20):** Click **TRAIN EMPTY** in the Web UI and leave the room for **7 seconds**.
3. **Train Corners (Profiles 0–19):** For each corner (`TL`, `TR`, `BL`, `BR`), record the 4 cardinal directions (`⬆️`, `➡️`, `⬇️`, `⬅️` standing still) and the center rotation profile (`🔄` rotating slowly in place).
4. All 21 profiles and noise masks are automatically saved to **NVS Flash (`sp2d_nvs`)** — you do not need to retrain after rebooting the device.

---

## 🆕 V2.0 Changelog

### 🚀 RF Injector & Networking (Core 0)
- **LAN-First Hybrid Injection Engine:** Added RFC-compliant `lan_dns_query` targeting the local gateway (`:53` and UDP `:33434..33437`). Maintains ~300 Hz PPS even when WAN/internet is completely disconnected.
- **Adaptive Rate Control:** Dynamic `ping_delay` (`2..10ms`) with 4-tick socket draining prevents lwIP mutex contention.
- **Non-Blocking Wi-Fi Watchdog:** Automatic background reconnection on router drops (>6s) without losing calibration state.

### 🛡️ Guard Security System (`STATE_GUARD`)
- **New 4th Main Menu Mode:** Added Guard Mode with a custom procedural vector shield HUD.
- **100% Silent Stealth Operation:** Hardware speaker is strictly muted during calibration, monitoring, and alerts.
- **18s Exit Grace Calibration:** 9s exit countdown (`LEAVE!`) + 6s baseline training + 3s noise floor & subcarrier quality profiling.
- **Screen Auto-Sleep & Wake Key Swallowing:** Display turns off after 15s idle; waking the screen with any key swallows the press-hold-release cycle to prevent accidental commands.
- **Leaky Integrator & Room Area ($m^2$) Scaling:** Eliminated through-wall false promotions ("7-second neighbor bug"). Added `AUTO` polygon area calculation and 10 manual presets (`2..45 m²`) via `[A]` and Web UI.
- **AGC Spike Rejector & Subcarrier Quality Mask:** Suppresses common-mode router Tx-power jumps and down-weights noisy subcarriers during calibration.
- **Live Coherence Bar & Confidence `%`:** Added real-time `0–100%` in-room energy bar on both the Cardputer HUD and Web UI, plus per-event confidence percentages (`WALL? 92%`, `IN-ROOM 96%`, `TAMPER 100%`).
- **Rain Mode (`[W]`) & IMU Tamper (`[M]`):** Added dielectric drift compensation for heavy rain and 8Hz 3D accelerometer/gyroscope anti-tamper protection.
- **NVS Event Logger, Offline RTC & CSV Export:** 32-event coalesced ring buffer in NVS Flash, manual `[T]` clock + automatic browser time sync, threat filter `[F]`, and 1-click `.CSV` export.

### 💾 NVS Persistence, Zero-Heap & CPU Optimizations (Core 1)
- **Full NVS Storage for 2D & SLAM:** Room polygons (`map_nvs`) and all 21 2D spatial profiles (`sp2d_nvs`) now persist across reboots. Fixed `[ESC]` bug that previously cleared mapped points.
- **Room Map Integration in Guard Web UI:** View your mapped room polygon, wall dimensions, and area directly inside the Guard Web-Terminal, or start/stop room mapping without leaving Guard Mode.
- **Zero Heap Fragmentation:** Replaced all `String` concatenations in WebServer handlers with static `snprintf` buffers (`json_buf`, `log_buf`) and added `pollBusy` request gating in JS.
- **14x Faster Math & Rendering:** Moved 32-subcarrier 2D projection to the 25Hz timer, added a 46-point trigonometric LUT in `draw1DMode()`, and expanded the on-device Manual `[I]` to **6 pages**.

---

## 💬 Feedback & Suggestions
If you find a bug or have an idea for the next release:
1. Open the **[Issues]** tab at the top of this repository.
2. Click **[New Issue]** and describe your setup, router model, and wall type.

## ☕ Support the Project
If this project has been useful for your security setup, hardware testing, or research, you can support further development here:
- **[https://boosty.to/zeloksa](https://boosty.to/zeloksa)**

---
*Created by Zeloksa. Optimized for M5Stack Cardputer & Cardputer ADV.*
