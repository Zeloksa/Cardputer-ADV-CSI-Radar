# 📡 Cardputer ADV CSI Radar (V1.42)

**Radar V1.42 ADV** is a high-performance Wi-Fi CSI (Channel State Information) sensing tool specifically optimized for the **M5Stack Cardputer ADV**. It utilizes advanced radio-wave analysis to detect motion and spatial presence through walls and obstacles.

> [!IMPORTANT]
> **Source Code Status:** This project is proprietary. The source code is private. 
> **Distribution:** Binary only via **M5Burner**.

---

## ⚡ Technical Highlights
* **Dual-Core Architecture:** Networking/Injection handled on Core 0, Math/UI on Core 1 for zero-lag performance.
* **200+ PPS (Packets Per Second):** Achieving the physical limit of the 2.4GHz Wi-Fi stack for high-fidelity sensing.
* **External NAT Bypass:** Custom injection engine that bypasses router flood protection for stable tracking.
* **2D Spatial Profiles:** Train up to 21 unique spatial zones for advanced environment monitoring.
* **Smart Power Management (New in V1.42):** Intelligent noise filtering for charging detection and manual wake-up protocols.

---

## 🛠 Installation
1. Open **M5Burner**.
2. Search for `CSI Radar` or `Zeloksa`.
3. Select version **V1.42 ADV**.
4. Burn to your M5Stack Cardputer.

---

## 🕹 Controls & Usage
* **[ENTER]**: Toggle Radar / Exit Energy Saving Mode.
* **[ D ]**: Toggle Web Server & IP Overlay.
* **[ R ]**: Toggle 1D Graph / 2D Spatial Mode.
* **[ H ]**: MAX Mode (333Hz / 240MHz CPU) / AUTO Mode.
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
