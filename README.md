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
* **Experimental 2D Spatial Profiles:** Support for training up to 21 unique spatial zones. 
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
  > [!TIP]
  > **MAX Mode** significantly increases detection precision and resolution by hitting 200+ PPS. 
  > *Warning: This mode results in faster battery drain and slight device heating due to high CPU and Wi-Fi load.*
* **[ , / . ]**: Manual Frequency (Hz) Adjustment.
* **[ W / S ]**: Volume Control.
* **[ [ / ] ]**: Screen Brightness.
* **[ DEL ]**: Factory Reset (Clear Wi-Fi credentials).

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
