# rtk-gnss-ins-field-setup
# Field Integration & Troubleshooting of MTi-680G RTK-GNSS/INS

## 📌 Overview
This repository documents the field configuration, trouble-shooting, and data collection procedures for the **Movella MTi-680G RTK-GNSS/INS** sensor unit integrated with an autonomous test vehicle.

---

## 🛠 Hardware & Communications Setup
* **Sensor**: Movella MTi-680G (Multi-band RTK-GNSS / INS)
* **Correction Data Source**: National Geographic Information Institute (NGII) NTRIP Caster (`rts1.ngii.go.kr:2101`)
* **Communication Interface**: RS232/USB via SENA SD1000 Bluetooth-Serial Adapter
* **Host Platform**: PC running MT Manager Software via Cellular Hotspot

---

## 🚨 Field Problem & Troubleshooting Log

### **Problem Statement**
During initial field deployment, the RTK filter failed to transition from `Float` to `Fixed` mode. Additionally, launching specific GUI diagnostic views (e.g., `Status` window) caused application crash, and position output exhibited severe multipath jitter (~3m error).

### **Root Cause Analysis**
1. **Signal Shadowing & Multipath**: The test cart was positioned too close to a multi-story stone building arcade, blocking line-of-sight (LOS) satellites.
2. **Insufficient Correction Satellites**: Diagnostic view showed high count of `Used` satellites (dark green), but insufficient `Used+D` satellites (light green) due to blocked RTCM correction channels.

### **Action & Solution**
1. **Relocation**: Moved the sensor system to an open-sky area (15m away from building structures) to eliminate signal occlusion.
2. **NTRIP Stream Reset**: Re-established socket connection to force update of RTCM3 stream (`Bytes forwarded` monitoring).
3. **Data Export Configuration**: Reconfigured MT Manager Output settings to **LLA (Latitude, Longitude, Altitude)** coordinate frame and exported time-series CSV with `StatusWord` for verification.

### **Result**
* Successfully achieved stable **RTK Fixed** state.
* Captured high-precision 3D trajectory (Latitude, Longitude, Altitude at ~138m MSL) for autonomous driving simulation.

---

## 📂 Exported Dataset Schema
The exported `.csv` / `.txt` log file contains the following primary fields:
| Field Name | Unit / Format | Description |
| :--- | :--- | :--- |
| `Latitude` | Degrees (DD.dddd) | WGS84 Geodetic Latitude |
| `Longitude` | Degrees (DDD.dddd) | WGS84 Geodetic Longitude |
| `Altitude` | Meters (m) | Height above Mean Sea Level (MSL) |
| `StatusWord` | Hex / Bitmask | Indicator for GNSS Fix status & RTK mode |
