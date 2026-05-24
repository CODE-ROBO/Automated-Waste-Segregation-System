# Industry-Sponsored Automated Waste Segregation System ♻️🏭

## 🛠️ Project Specifications
* **Project Partner:** Janatics India (Industrial Automation Skill Challenge)
* **Core Domains:** Industrial Automation, Hybrid Sensor Fusion, PLC Programming (Ladder Logic), Pneumatic/Actuator Control
* **Status:** Fully Functional Prototype / Validated System Architecture

---

## 📌 Technical Overview & Industrial Architecture
This repository contains the system configurations, control logic, and architectural frameworks for an automated, multi-material industrial segregation system. Developed as an industry-sponsored project in collaboration with **Janatics India**, the system replaces slow, error-prone manual sorting with an automated, high-throughput mechatronic sorting line.

The system uses a **hybrid sensor-fusion array**—combining Inductive, Capacitive, and Optical sensors. By reading multiple physical properties of an object simultaneously, the controller eliminates material ambiguities in real-time, executing high-speed sorting commands via synchronized industrial actuators.

### 🎯 Key Engineering Achievements
* **Hybrid Sensor-Fusion Matrix:** Designed an optimized sensor array to read metallic, non-metallic, organic, and moisture-based characteristics within a sub-150ms processing window.
* **Industrial PLC Control Loop:** Programmed the master control sequence using **Siemens PLC Ladder Logic**, ensuring rugged, deterministic, 24/7 runtime operational stability.
* **Actuator Synchronization:** Interfaced low-latency sensor triggers with physical sorting mechanisms (pneumatic ejectors/servo gates) to execute clean material routing without jamming the conveyor feed line.

---

## 📂 Repository Architecture
* **`/plc`**: Contains exported Siemens PLC Ladder Logic sheets, state-machine configurations, and I/O memory maps.
* **`/hardware`**: Contains industrial wiring schematics, panel layouts, sensor bracket CAD designs, and pneumatic plumbing layouts.
* **`/docs`**: Contains sensor response calibration tables, timing optimization logs, and the structural system overview.

---

## 📋 Development & Validation Milestones
- [x] Mechanical frame assembly, conveyor belt setup, and actuator alignment
- [x] Electrical routing, industrial grounding, and multi-sensor array power delivery
- [x] Empirical logging of sensor output thresholds for distinct material batches
- [x] Development and optimization of the core Siemens PLC Ladder Logic state machine
- [x] Closed-loop integration: Synchronizing high-speed material sorting gates with sensor-matrix triggers
- [x] Endurance testing under heavy, multi-material conveyor load distributions
- [x] Project completion and technical presentation showcase for the industrial challenge

---

> *Note: Code snippets, memory allocations, and hardware layouts are open-sourced within their respective directories for educational use and reference.*
