# Unified Autonomous Driving System (ACC + LKA Simulation)

## Overview

This project implements a simplified autonomous driving stack integrating:

* Adaptive Cruise Control (ACC)
* Lane Keeping Assist (LKA)
* Sensor Fusion
* Behavior Planning

The system simulates a vehicle following a lead car while maintaining a safe distance and lane position.
Both **PID-based control** and **rule-based control** are implemented and compared.

---

## System Architecture

```
Perception → Fusion → Planning → Control → Simulation
```

### Modules

* **simulation/**

  * Vehicle dynamics and environment update

* **perception/**

  * Simulated sensors (camera + radar with noise)

* **system/**

  * Sensor fusion (weighted average)

* **planning/**

  * Behavior decision (CRUISE / FOLLOW / SLOW_DOWN / EMERGENCY_BRAKE)
  * TTC-based logic with state smoothing

* **control/**

  * ACC (PID + Rule-based)
  * LKA (proportional steering)

---

## Key Features

### 1. Adaptive Cruise Control (PID)

* Time headway based distance control
* PID controller with:

  * Integral windup protection
  * Derivative smoothing
  * State-dependent gain (FOLLOW 강화)

### 2. Behavior Planning

* TTC (Time-To-Collision) 기반 상태 전환
* State smoothing으로 잦은 상태 변경 방지

### 3. Sensor Fusion

* Radar + Camera 거리 데이터를 가중 평균으로 통합

### 4. Performance Comparison

* PID vs Rule-based control 비교
* Metrics:

  * Mean Distance Error
  * Minimum TTC (Safety)
  * Speed Smoothness

---

## Results

### 1. Vehicle State Visualization

* Speed / Distance / TTC
* Behavior segmentation (background color)

### 2. PID vs Rule Comparison

* PID shows smoother response and improved tracking stability
* Rule-based control reacts more abruptly and lacks stability

---

## Challenges

### 1. PID Tuning Difficulty

Initial PID parameters resulted in:

* Distance continuously decreasing
* No clear improvement over rule-based control

Tuning required balancing:

* Responsiveness (Kp)
* Stability (Kd)
* Long-term error correction (Ki)

---

### 2. State Oscillation

Behavior switching was unstable due to:

* Sensor noise
* TTC sensitivity

Solution:

* Introduced **minimum state duration**
* Reduced rapid oscillation between states

---

### 3. Sensor Noise Impact

Noise from simulated sensors caused:

* TTC spikes
* Unstable control behavior

Solution:

* Weighted sensor fusion
* Derivative smoothing in PID

---

## Issues Encountered

* **Matplotlib rendering issue (Colab)**
  Multiple graphs not displaying correctly
  → Fixed with explicit figure separation and `plt.close()`

* **PID state persistence bug**
  Integral/derivative states were shared across simulations
  → Reset at each run

* **Incorrect derivative smoothing implementation**
  → Fixed using filtered derivative instead of raw error reuse

---

## Limitations

### 1. Simplified Vehicle Dynamics

* No real physics model (no drag, no tire model)
* Instant acceleration response

---

### 2. Basic Sensor Model

* Random noise only
* No latency, occlusion, or failure cases

---

### 3. Heuristic Behavior Planning

* Threshold-based logic
* No predictive or optimization-based planning

---

### 4. Limited Control Scope

* Only longitudinal control (ACC) is sophisticated
* LKA is simple proportional control

---

### 5. No Real-world Validation

* Entire system is simulation-based
* No real vehicle or dataset validation


---

## How to Run

```bash
python main.py
```

Outputs:

* `simulation_result.png`
* `comparison_result.png`
* `pid_log.csv`
* `rule_log.csv`

---

## Key Takeaway

This project demonstrates:

* End-to-end autonomous driving pipeline understanding
* Practical implementation of control systems (PID vs Rule)
* Ability to analyze system behavior and identify limitations

The focus is not just implementation, but **understanding trade-offs between control strategies and system stability**.
