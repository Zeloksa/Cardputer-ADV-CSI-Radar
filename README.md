![Version](https://img.shields.io/badge/Version-1.42_ADV-blue)
![Hardware](https://img.shields.io/badge/Hardware-Cardputer-orange)
![Platform](https://img.shields.io/badge/Platform-M5Stack-red)
![License](https://img.shields.io/badge/License-Proprietary-gray)
[![Boosty](https://img.shields.io/badge/Support-Boosty-orange)](https://boosty.to/zeloksa)
# 📡 WiFi CSI Radar ADV (V1.42)

**Radar V1.42 ADV** is a high-performance Wi-Fi CSI (Channel State Information) sensing tool specifically optimized for the **M5Stack Cardputer ADV**. It utilizes advanced radio-wave analysis to detect motion and spatial presence through walls and obstacles.

> [!IMPORTANT]
> **Source Code Status:** This project is proprietary. The source code is private. 
> **Distribution:** Binary only via **M5Burner**.

---

## ⚡ Technical Highlights

* **Dual-Core FreeRTOS Architecture:** Networking and high-speed injection are handled on Core 0, while heavy Math and UI rendering run on Core 1 for zero-lag, real-time sensing.
* **Ultra-Precise 1D Sensing Core:** The original and most accurate engine. Specifically tuned for high-fidelity motion detection, capable of sensing subtle movements (like breathing or micro-vibrations) through walls.
* **Hybrid Room Mapping (SLAM):** Integrated logic using the onboard IMU to create a digital layout of your room. This allows for localized tracking within a mapped environment.
* **Experimental 2D Spatial Profiles**: Support for training up to 21 unique spatial zones.

> [!NOTE]
> 2D Mode is currently in **Active Beta**. Accuracy is being refined, and I am actively looking for algorithmic suggestions and community feedback to improve spatial triangulation.
* **200+ PPS (Packets Per Second):** Achieving the physical limit of the 2.4GHz Wi-Fi stack. Higher PPS equals higher resolution data for motion analysis.
* **External NAT Bypass:** A custom injection engine that routes traffic through external DNS targets to bypass router flood protection and rate-limiting.
* **ADV Smart Power Management:** Intelligent noise filtering for charging detection (10s verification) and manual wake-up protocols to prevent interference during high-load sensing.

---

## 🛠 Installation
1. Open **M5Burner**.
2. Search for `WiFi CSI Radar ADV` or `Zeloksa`.
3. Select version **V1.42 ADV**.
4. Burn to your M5Stack Cardputer.

---

## 🕹 Controls & Usage
* **[ENTER]**: Toggle Radar / Exit Energy Saving Mode.
* **[ D ]**: Toggle Web Server & IP Overlay.
* **[ R ]**: Toggle 1D Graph / 2D Spatial Mode.
* **[ H ]**: **MAX Mode** (333Hz / 240MHz CPU) / AUTO Mode.
* **[ , / . ]**: Manual Frequency (Hz) Adjustment.
* **[ W / S ]**: Volume Control.
* **[ [ / ] ]**: Screen Brightness.
* **[ DEL ]**: Factory Reset (Clear Wi-Fi credentials).
## 📖 Comprehensive User Manual

To get the most accurate readings from the CSI Radar, you must understand how Wi-Fi signals bounce in your environment. Follow these steps for each mode:

### 🎯 1D Mode: High-Precision Motion Detection
This is the default mode. It acts as an ultra-sensitive invisible tripwire.
1. **Positioning:** Place the Cardputer on a stable, flat surface. **Do not hold it in your hands.**
2. **Start Calibration:** Press `[ENTER]`. The screen will show a `Cal: 12s` countdown.
3. **The "Ghost" Rule:** Immediately step away from the device and stand perfectly still, or leave the room. The radar needs these 12 seconds to learn the "static baseline" of the empty space.
4. **Armed State:** Once the countdown disappears, the alarm is armed (`Alm:ON`). Any movement (breathing, walking) will disrupt the CSI baseline, spike the graph, and trigger the alarm.

### 🗺️ 2D Mode: Spatial Profiles (Active Beta)
This mode attempts to locate *where* the movement is happening using the Web UI.
1. Switch to 2D Mode by pressing `[ R ]`.
2. Press `[ D ]` to enable the Web Server and note the IP address on the screen. Open this IP in your smartphone or PC browser.
3. **Train the Void (Crucial):** In the Web UI, click **TRAIN EMPTY**. **Everyone must immediately leave the room** for 7 seconds. The radar must memorize the completely empty room (Zone 20).
4. **Train the Corners:** Once the room is memorized, stand in the Top-Left (TL) corner of the room with your phone. Click `TL Corner` in the UI and stand perfectly still. Repeat this for all 4 corners. 
5. The red dot on the Web UI will now attempt to interpolate your physical position based on these anchors.

### 📍 Hybrid SLAM: Room Mapping
Use the onboard IMU to draw a digital floor plan of your room.
1. In the Web UI (while in 2D mode), click **START ROOM MAPPING**.
2. **Posture:** Hold the Cardputer perfectly flat against your chest, screen facing outward.
3. **The Walk:** Walk at a steady, robotic pace. Step firmly on your **HEEL** with each step (the IMU uses the physical shock to count steps). 
4. **Turns:** When you reach a corner, stop, turn exactly 90 degrees on the spot, and continue walking.
5. You will hear a beep for every successfully registered step. Click STOP in the Web UI when you have closed the loop.

> [!WARNING]
> **Environmental Variables:** Wi-Fi CSI is highly sensitive. Moving furniture, opening doors, or even heavy rain outside can alter the baseline. Always recalibrate the 1D or 2D profiles if the environment changes.

> [!TIP]
> **MAX Mode** significantly increases detection precision and resolution by hitting 200+ PPS.
> *Warning: This mode results in faster battery drain and slight device heating due to high CPU and Wi-Fi load.*

---

## 🆕 V1.42 ADV Changelog
* **Improved Charging Logic:** Integrated a 10-second verification timer to eliminate "ghost" charging notifications caused by Wi-Fi power noise.
* **Manual Wake-up:** Device stays in sleep mode during/after charging until the owner presses **[ENTER]**.
* **High-Speed Buffer:** Implemented FreeRTOS RingBuffer to prevent packet loss during heavy UI drawing.

---

## 💬 Feedback & Suggestions
If you find a bug or have a suggestion for the next version:
1. Go to the **[Issues]** tab at the top of this page.
2. Click **[New Issue]**.
3. Describe your problem or idea in detail.

---

## ☕ Support the Project
If this tool has been useful for your research or hobbyist projects, consider supporting further development:
* **[https://boosty.to/zeloksa]**

---
*Created by Zeloksa. Optimized for Cardputer ADV.*
