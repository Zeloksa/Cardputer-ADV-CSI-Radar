![Version](https://img.shields.io/badge/Version-1.5-blue)
![Hardware](https://img.shields.io/badge/Hardware-Cardputer-orange)
![Platform](https://img.shields.io/badge/Platform-M5Stack-red)
![License](https://img.shields.io/badge/License-Proprietary-gray)
[![Boosty](https://img.shields.io/badge/Support-Boosty-orange)](https://boosty.to/zeloksa)

# 📡 WiFi CSI Radar (V1.5)

**Radar V1.5** is a high-performance Wi-Fi CSI (Channel State Information) sensing tool specifically optimized for the **M5Stack Cardputer**. It utilizes advanced radio-wave analysis to detect motion and spatial presence through walls and obstacles.

> [!IMPORTANT]
> **Source Code Status:** This project is proprietary. The source code is private. 
> **Distribution:** Binary only via **M5Burner**.

---

## ⚡ Technical Highlights

* **Dual-Core FreeRTOS Architecture:** Networking and high-speed injection are handled on Core 0, while heavy Math and UI rendering run on Core 1 for zero-lag, real-time sensing.
* **Ultra-Precise 1D Sensing Core:** The original and most accurate engine. Specifically tuned for high-fidelity motion detection, capable of sensing subtle movements (like breathing or micro-vibrations) through walls.
* **Hybrid Room Mapping (SLAM) with Smart Geometry:** Integrated IMU logic to create a digital layout of your room. Features an Angle-Based Segment Merging engine that mathematically aggregates collinear steps into perfectly straight walls and automatically trims/closes polygon gaps.
* **2D Spatial Profiles**: Support for training up to 21 unique spatial zones.

> [!NOTE]
> 2D Mode is currently in **Active Beta**. Accuracy is being refined. The system now features an expanded **135x135px** geometric tracking grid if no room is mapped.

* **External NAT Bypass:** A custom injection engine that routes traffic through external DNS targets to bypass router flood protection and rate-limiting.
* **Deep SAFE MODE:** Intelligent power management. When charging, the device drops CPU frequency to 80MHz, suspends IMU polling, and halts all heavy CSI processing to ensure safe, cool, and rapid charging.

---

## 🛠 Installation
1. Open **M5Burner**.
2. Search for `WiFi CSI Radar` or `Zeloksa`.
3. Select version **V1.5**.
4. Burn to your M5Stack Cardputer.

---

## 🕹 Controls & Usage

* **[ ENTER ]**: Toggle Radar / Start Calibration / Hold to Reset Map
* **[ i ]**: Open Interactive User Manual (3 Pages)
* **[ D ]**: Toggle Web Server & IP Overlay
* **[ R ]**: Switch between 1D Radar / 2D Spatial Mode
* **[ H ]**: **MAX Mode** (Force 240MHz CPU & Maximum Polling) / AUTO Mode
* **[ , / . ]**: Manual Frequency (Hz) Adjustment
* **[ - / = ]**: Volume Control (Base volume normalized to 30%)
* **[ [ / ] ]**: Hardware Screen Brightness Control
* **[ ESC / \` ]**: Back / Exit Current Mode
* **[ DEL ]**: Factory Reset (Clear Wi-Fi credentials, works in Main Menu & Boot)

---

## 📖 Comprehensive User Manual

To get the most accurate readings from the CSI Radar, you must understand how Wi-Fi signals bounce in your environment. Follow these steps for each mode:

### 🎯 1D Mode: High-Precision Motion Detection
This is the default mode. It acts as an ultra-sensitive invisible tripwire.
1. **Positioning:** Place the Cardputer on a stable, flat surface. **Do not hold it in your hands.**
2. **Start Calibration:** Press `[ENTER]`. The screen will show a 12s countdown.
3. **The "Ghost" Rule:** Immediately step away from the device and stand perfectly still, or leave the room. The radar needs these 12 seconds to learn the "static baseline" of the empty space.
4. **Armed State:** Once the countdown disappears, the alarm is armed. Any movement (breathing, walking) will disrupt the CSI baseline, spike the graph, and trigger the alarm.

### 🗺️ 2D Mode: Spatial Profiles (Active Beta)
This mode attempts to locate *where* the movement is happening using the Web UI.
1. Switch to **2D Mode** by pressing `[ R ]`.
2. Press `[ D ]` to enable the Web Server and note the IP address on the screen. Open this IP in your smartphone or PC browser.
3. **Train the Void (Crucial):** In the Web UI, click **TRAIN EMPTY**. **Everyone must immediately leave the room** for 7 seconds. The radar must memorize the completely empty room baseline (Profile 20).
4. **Train the Corners (5-Step Profiling):** The radar needs to learn your Wi-Fi reflection from multiple angles. Go to the Top-Left (TL) corner of your room and open the TL menu in the Web UI. You must record 5 states for this corner:
    * **Arrows (⬆️ ⬇️ ⬅️ ➡️):** Click an arrow and stand perfectly still facing that direction for 7 seconds.
    * **Center Button (🔄):** Click the center icon, and **rotate slowly 2 times in place** while the radar beeps.
5. Repeat this complete 5-step process for all 4 corners of the room (TL, TR, BL, BR). 
6. Once all 20 spatial profiles are trained, the tracking dot on the Cardputer and Web UI will interpolate and track your physical position in real-time.

### 📍 Hybrid SLAM: Room Mapping
Use the onboard IMU to draw a digital floor plan of your room.
1. Switch to Mapping Mode via the main menu. Follow the on-screen instructions.
2. **Posture:** Hold the Cardputer perfectly flat against your chest, screen facing outward.
3. **The Walk:** Press `[ENTER]`, then walk at a steady, robotic pace. Step firmly on your **HEEL** with each step (the IMU uses the physical shock to count steps). 
4. **Turns:** When you reach a corner, stop, turn exactly 90 degrees on the spot, and continue walking.
5. You will hear a beep for every successfully registered step. Click STOP in the Web UI (or press `[ENTER]`) when you have closed the loop. The Smart Geometry engine will automatically calculate the wall lengths (**m**) and total room area (**m²**).

> [!WARNING]
> **Environmental Variables:** Wi-Fi CSI is highly sensitive. Moving furniture, opening doors, or even heavy rain outside can alter the baseline. Always recalibrate the 1D or 2D profiles if the environment changes.

> [!TIP]
> **MAX Mode** significantly increases detection precision and resolution.
> *Warning: This mode results in faster battery drain and slight device heating due to high CPU and Wi-Fi load.*

---

## 🆕 V1.5 Changelog

* **Network & Boot UI Overhaul:**
    * Completely redesigned the Wi-Fi connection sequence. Added a stylized "Scanning Networks" boot animation and full-screen grid backgrounds.
    * Introduced a scrollable UI list for nearby Wi-Fi networks (view all available networks, not just the top 4).
    * Implemented a smooth scrolling text animation (marquee) for long SSIDs during WPA2 key entry.
    * Redesigned connection error screens ("SYSTEM ALERT", "LINK FAILED") with professional, readable layouts.
    * Added the ability to press `[ESC]` to back out of the password entry screen and return to the network scan list.
* **1D Radar Enhancements:**
    * Replaced the standard 1D drawing logic with immersive "submarine-style" expanding radar wave animations for better visual feedback.
* **2D & Mapping Improvements:**
    * Expanded the 2D tracking grid to 135x135 pixels, taking full advantage of the freed vertical screen space.
    * Added clear, on-screen text instructions directly inside the Room Mapping standby mode so users know exactly how to walk.
    * Fixed the Web-UI "SWITCH TO 1D/2D" button logic to properly sync and trigger the state machine on the Cardputer.
* **System & Quality of Life (QoL) Fixes:**
    * **Removed Auto-Dimming:** The screen no longer forces low brightness when the Web-UI is active. Screen brightness is now fully user-controlled `[ / ]` at all times.
    * **Reliable Factory Reset:** Fixed the `[DEL]` key logic to ensure a definitive wipe of saved Wi-Fi credentials and a clean reboot. This now works safely from both the Main Menu and the Boot Screen.
    * **Cleaned up Main Menu:** Removed horizontal separator lines for a sleeker, uninterrupted fullscreen grid appearance.

---

## 💬 Feedback & Suggestions
If you find a bug or have a suggestion for the next version:
1. Go to the **[Issues]** tab at the top of this page.
2. Click **[New Issue]**.
3. Describe your problem or idea in detail.

---

## ☕ Support the Project
If this tool has been useful for your research or hobbyist projects, consider supporting further development:
* **[https://boosty.to/zeloksa](https://boosty.to/zeloksa)**

---
*Created by Zeloksa. Optimized for Cardputer.*
