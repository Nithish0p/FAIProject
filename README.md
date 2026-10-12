# RedTaxi Spatial-Temporal Anomaly Detection (ST-HDBSCAN)

This repository contains the Python simulation code for an unsupervised machine-learning architecture designed to passively infer physical road blockages using ride-hailing fleet GPS telemetry. It separates legitimate collective detours from isolated or coordinated driver fraud without requiring manual human review.

## 📌 Problem Statement
Ride-hailing platforms like Uber, Ola, and RedTaxi enforce strict compliance with pre-calculated route polygons to prevent fare-inflation scams. However, when a road is physically blocked by an unmapped event, drivers taking a legitimate detour are incorrectly penalized. This system mathematically identifies real blockages to automatically mute penalty warnings and update the global navigation SDK.

## 🧠 System Architecture
The system clusters off-route GPS telemetry with Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN). A suspected detour is accepted only if it successfully passes a strict three-gate logic sequence:

1. **Gate 1 (Spatial Density):** Requires a minimum spatial vehicle density of `min_cluster_size >= 10`. Isolated scammers are instantly labeled as Noise (-1).
2. **Gate 2 (Temporal Window):** Defends against "slow-drip" data poisoning by enforcing a maximum time span of $\Delta t \le 3.0$ minutes. The time dimension is multiplied by a weight of 5.0 during preprocessing to mathematically sever delayed attackers.
3. **Gate 3 (Velocity Cross-Check):** Queries external telemetry to confirm the main road speed has dropped below 15 km/h.

## 📊 Simulation Scenario
The code runs a synthetic 230-vehicle, 30-minute simulation mapped to a localized traffic grid in Gandhipuram, Coimbatore. The dataset includes:
* **200 Main-Route Vehicles:** Background traffic.
* **15 Valid Detour Vehicles:** Genuine blockage response arriving in a tight 2-minute window.
* **10 Slow-Drip Attackers:** Fraudulent drivers perfectly mimicking the detour's path, but artificially spaced out over 30 minutes.
* **5 Rogue Outliers:** Isolated, scattered fraud attempts.

## ⚙️ Installation & Requirements
The simulation requires Python 3 and the following dependencies:

```bash
pip install numpy pandas scikit-learn hdbscan plotly
