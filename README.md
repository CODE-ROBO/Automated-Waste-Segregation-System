<h1 align="center">Automated Waste Segregation System ♻️🏭</h1>
<h4 align="center">Industrial Hybrid Sensor-Fusion, PLC Ladder Logic, & High-Throughput Mechatronics</h4>

<p align="center">
  <br>
  <img src="https://img.shields.io/badge/Siemens_TIA_Portal-FFD700?style=for-the-badge&logo=siemens&logoColor=black" alt="Siemens TIA Portal"/>
  <img src="https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white" alt="C++"/>
  <img src="https://img.shields.io/badge/Fusion360-00FFFF?style=for-the-badge&logo=Fusion360&logoColor=black" alt="Fusion360"/>
  <img src="https://img.shields.io/badge/Altium-8B0000?style=for-the-badge&logo=altium&logoColor=white" alt="Altium"/>
  <img src="https://img.shields.io/badge/Siemens_TIA_Portal-FFD700?style=for-the-badge&logo=siemens&logoColor=black" alt="Siemens 
</p>
<p align="center">
  <img src=""C:\Users\harsh\OneDrive\Desktop\3D Design\Automated Waste Segregation ( JASC )\Assembly.jpeg"" alt="Waste Segregation Hardware Render" width="100%"/>
</p>

<details open>
  <summary><b>📑 DIRECTORY TERMINAL (TABLE OF CONTENTS)</b></summary>
  <ol>
    <li><a href="#overview">Executive System Overview</a></li>
    <li><a href="#datasheet">System Datasheet & Engineering Targets</a></li>
    <li><a href="#logic">Hybrid Sensor Fusion & Boolean Control Logic</a></li>
    <li><a href="#hardware">Hardware Fabrication & Bill of Materials</a></li>
    <li><a href="#pipeline">Actuator Synchronization & PLC Pipeline</a></li>
    <li><a href="#architecture">Repository Architecture & CI/CD</a></li>
    <li><a href="#validation">Empirical Validation Matrix (Development Log)</a></li>
    <li><a href="#deployment">Deployment & Reproducibility</a></li>
    <li><a href="#team">Industrial Partnership & Janatics Challenge</a></li>
    <li><a href="#academic">Academic Trajectory</a></li>
    <li><a href="#citation">Academic Citation</a></li>
  </ol>
</details>
---

### <a id="overview"></a>🌐 EXECUTIVE SYSTEM OVERVIEW

<div align="justify">
This repository contains the system configurations, control logic, and architectural frameworks for an automated, multi-material industrial segregation system. Developed as an industry-sponsored build for the <b>Janatics India Automation Skill Challenge</b>, the system bridges the gap between theoretical mechatronics and 24/7 rugged industrial deployment, replacing slow, error-prone manual sorting with an automated, high-throughput mechatronic sorting line.

By rejecting standard single-sensor models, this architecture utilizes a <b>hybrid sensor-fusion array</b> (Inductive, Capacitive, Optical) paired with microcontrollers for preprocessing. By reading multiple physical properties of an object simultaneously, the Siemens PLC controller eliminates material ambiguities in real-time, executing high-speed, sub-150ms sorting commands via synchronized pneumatic actuators.
</div>

---

### <a id="datasheet"></a>📋 SYSTEM DATASHEET & ENGINEERING TARGETS

<div align="justify">
The architecture focuses on deterministic latency, ensuring the physical actuators trigger precisely as the targeted material enters the ejection zone.
</div>

| Subsystem | Specification | Engineering Objective |
| :--- | :--- | :--- |
| **Control Architecture** | Siemens S7 / TIA Portal Ladder Logic | Rugged, deterministic, 24/7 continuous runtime stability. |
| **Sensor Matrix** | Optical, Capacitive, & Inductive Array | Multi-modal material identification (Metal, Plastic, Organic). |
| **Actuator Latency** | $< 150\text{ms}$ Response Window | High-speed pneumatic ejection without conveyor line jamming. |
| **Preprocessing** | Microcontroller ADC Filtering | Smooth raw sensor analog voltages before PLC ingestion. |
| **Sorting Mechanisms** | Fast-Acting Pneumatic Solenoids / Ejectors | Clean trajectory routing into designated material bins. |
| **Project Status** | **PROTOTYPE VALIDATED** | Closed-loop integration and heavy-load endurance confirmed. |

---

### <a id="logic"></a>🧠 HYBRID SENSOR FUSION & BOOLEAN CONTROL LOGIC

<div align="justify">
To achieve near-perfect sorting accuracy, the system relies on overlapping sensor detection zones. The PLC evaluates a Boolean logic state machine to determine material composition. Let $S_{ind}$ represent the Inductive Sensor (detects metals), $S_{cap}$ represent the Capacitive Sensor (detects dense non-metals/moisture), and $S_{opt}$ represent the Optical Break-Beam (detects object presence).
</div>

The system executes the following real-time logic gates for material classification:

**Metallic Material Identification:**
$$M_{metal} = S_{ind} \land S_{opt}$$

**Dense Plastic / Moisture-heavy Organic Identification:**
$$M_{plastic/organic} = \neg S_{ind} \land S_{cap} \land S_{opt}$$

**Low-Density / Dry Non-Metal Identification:**
$$M_{dry\_waste} = \neg S_{ind} \land \neg S_{cap} \land S_{opt}$$

<div align="justify">
Once a Boolean state evaluates to True, the PLC initiates a highly calibrated timer block ($T_{delay}$), calculated by the conveyor velocity ($v$) and the distance to the ejection gate ($d$), ensuring the pneumatic solenoid ($A_{eject}$) fires at the exact millisecond of arrival:
</div>

$$T_{delay} = \frac{d}{v}$$

---

### <a id="hardware"></a>⚙️ HARDWARE FABRICATION & BILL OF MATERIALS

<div align="justify">
Built entirely from scratch, the physical structure had to withstand the continuous mechanical vibration of industrial conveyor motors and high-PSI pneumatic actuation without sensor misalignment.
</div>

* 🛡️ **Frame Dynamics:** Rigid multi-axis mechanical frame assembly designed for rapid conveyor belt maintenance and modular actuator alignment.
* ⚡ **Electrical Routing:** Industrial-grade grounding architecture isolating high-voltage motor EMF from the low-voltage sensor matrix to prevent false positive triggers.
* 🌬️ **Pneumatic Plumbing:** Optimized pneumatic hose lengths and localized manifolds to reduce air-pressure drop and maximize solenoid punch speed.

**Primary Hardware Bill of Materials (BOM):**
| Component | Material / Specification | Subsystem |
| :--- | :--- | :--- |
| **Master Controller** | Siemens S7-1200 PLC | Central Logic Engine |
| **Sensor Array 1** | 12V Inductive Proximity Sensor | Metal Detection |
| **Sensor Array 2** | Adjustable Capacitive Sensor | Density / Moisture Detection |
| **Actuation Gates** | 5/2 Way Pneumatic Solenoid Valves | Physical Ejection Routing |
| **Transport** | High-Torque DC Gear Motor + Belt | Main Conveyor Line |

---

### <a id="pipeline"></a>📡 ACTUATOR SYNCHRONIZATION & PLC PIPELINE

<div align="justify">
A standard software loop is insufficient for industrial sorting; PLC Ladder Logic provides deterministic, cyclic execution. 
</div>

1. **Hardware Ingestion:** The multi-sensor array feeds continuous 24V discrete signals into the PLC input registers. 
2. **State Machine Execution:** The Ladder Logic evaluates the Boolean material states against an active shift register, tracking the physical location of the object as it moves down the belt.
3. **Actuator Trigger:** The corresponding pneumatic gate fires. The system logic includes mechanical retraction timers to prevent subsequent items from colliding with an open sorting gate.

---

### <a id="architecture"></a>🗄️ REPOSITORY ARCHITECTURE & CI/CD

<div align="justify">
<i>Structured for absolute transparency, providing the complete transition from raw electrical schematics to compiled logic blocks.</i>
</div>

```text
📁 Automated-Waste-Segregation/
│
├── 📁 .github/workflows/     # CI/CD: Automated linting for any Python/C++ preprocessing scripts
├── 📁 plc/                   # Central Logic Engine
│   ├── main_state_machine.ap16  # Exported Siemens TIA Portal Project
│   └── memory_mapping.csv       # IO Tag assignments
│
├── 📁 hardware/              # Physical Build Assets
│   ├── schematics/           # Altium/KiCad wiring diagrams and industrial grounding plans
│   ├── CAD_models/           # Custom sensor mounting brackets (STEP/STL)
│   └── pneumatics_layout.pdf # Air line routing and manifold distribution
│
├── 📁 docs/                  # System documentation
│   ├── sensor_calibration.xlsx # Empirical logs of sensor thresholds for distinct materials
│   └── SOP_startup.pdf       # Standard Operating Procedure for industrial power-on
│
├── requirements.txt          # Python dependencies for offline data analysis (if applicable)
└── README.md                 # Main system dossier
