# Project-Escapology
# Project Escapology: Heuristic Failsafes in Autonomous Navigation

**Author:** Kabeer  
**Date:** February 15, 2026  
**Status:** Proof of Concept (PoC) - Python

## 🛑 The Problem: The "Roundabout Trap"
A known critical vulnerability in autonomous vehicle (AV) navigation systems is the susceptibility to infinite behavioral loops. When short-term sensor data (e.g., lane keeping, obstacle avoidance) perfectly balances in a closed loop, an AV can become indefinitely trapped in structures like roundabouts. The system fails to recognize that a compilation of perfect micro-decisions has resulted in zero macro-progression.

## 💡 The Solution: The Heuristic Backdoor
Project Escapology introduces a secondary "Watchdog" algorithm that operates outside the primary navigation stack. It monitors spatio-temporal progression using a 3D displacement matrix over rolling time windows. 

Crucially, the algorithm mathematically differentiates between an **Infinite Loop** (a trap) and a **Curly Loop** (valid complex topologies, such as a spiral parking garage) by evaluating the Z-axis (altitude) against the net X/Y displacement.

### How it Works:
1. **Tracks Coordinates:** Logs the vehicle's X, Y, Z, and Time data.
2. **Net Displacement Check:** Compares current coordinates with historical coordinates from *n* seconds ago.
3. **The Escapology Logic:** * If `XY displacement ≈ 0` AND `Z displacement ≈ 0` AND `Time > threshold` ➡️ **Infinite Loop Detected. Halt and Recalculate.**
   * If `XY displacement ≈ 0` AND `Z displacement > threshold` ➡️ **Curly Loop Detected (e.g., Spiral Ramp). Safe to proceed.**

## 🚀 Running the Proof of Concept (PoC)
This repository contains the initial Python PoC demonstrating the core logic of the Heuristic Backdoor. 

### Prerequisites
* Python 3.x

### Execution
Run the `main.py` script. The console will output two simulated scenarios:
1. **Scenario 1:** A vehicle navigating a spiral garage. The watchdog successfully recognizes the elevation change and allows the vehicle to proceed.
2. **Scenario 2:** A vehicle trapped in a flat roundabout. The watchdog detects the net-zero displacement over time, activates the backdoor, and halts the system to prevent an infinite loop.

## 🗺️ Future Roadmap
* [x] Phase 1: Python text-based logic proof.
* [ ] Phase 2: Translation to C++ and integration with ROS 2 (Robot Operating System).
* [ ] Phase 3: 3D physics simulation using Gazebo/CARLA to test the backdoor against simulated LiDAR and Odometry drift.
