# Automatic Rotation System — Industrial PLC Automation

> **PLC-controlled automated rotary filling system** built from scratch during an industrial internship at Neotronik Automation.
---

## Project Overview

This project implements a fully automated **rotary filling system** designed to replace a manual industrial process. A conveyor belt feeds screws into 8 rotating tanks mounted on a gear-driven assembly. The PLC software manages the entire filling cycle autonomously, advancing to the next tank when one is full, and stopping safely when all tanks reach capacity.

---

##  System Architecture

### Physical Components

- **Siemens S7-1200 PLC** — main controller
- **Siemens KTP400 Basic HMI** — operator interface
- **2× OJ5126 Barrier Sensors** — fill-level detection
- **3× IE5338 Inductive Position Sensors** — precise rotary positioning
- **Three-phase motor**  + frequency inverter
- **Three-phase circuit breaker, contactor, relays**
- **24VDC power supply** (three-phase input)
- **Control panel**: 3-position selector switch, warning LED, RESET button, emergency mushroom button

### Software Structure (TIA Portal)

```
├── Main (OB1)           — main cyclic execution block
├── Initial_Call         — initialization logic
├── Maintenance          — system maintenance routines
├── Motor (FB)           — motor control function block
├── Conveyor (FB)        — conveyor belt logic
├── Sensors (FB)         — sensor input processing
├── Work_Mode (FB)       — mode management (Auto/Manual/Stop)
└── HMI Screen           — operator interface with real-time status
```

---

##  Operating Modes

###  AUTOMATIC Mode
Full autonomous operation. The system:
1. Starts the conveyor to fill the active tank
2. Monitors fill level via OJ5126 barrier sensor (5-second confirmation timer)
3. Validates drain-plate position with IE5338 sensor before rotating
4. Advances the rotary assembly to the next empty tank
5. Repeats until all 8 tanks are full — then stops and activates the warning LED

###  MANUAL Mode
Operator-controlled tank rotation for emptying. The conveyor is **disabled** for safety. Operator manually advances each tank one position at a time using the rotate button. Position changes are validated by inductive sensors before movement is allowed.

###  STOP Mode
Complete system halt triggered by:
- Selector switch set to STOP position
- All tanks full (automatic stop)
- Emergency button pressed

A RESET is required before transitioning to any other mode.

---

##  Safety Features

- **Emergency stop** (mushroom-type button) — immediate full shutdown
- **Inductive position validation** before every motor movement
- **5-second fill confirmation** to prevent false positives from sensor bounce
- **Conveyor lockout** in Manual mode — prevents accidental filling during emptying
- **Reset-required transitions** between operating modes

---

##  HMI Interface

The KTP400 HMI screen provides real-time visibility into:

- Live sensor states (filling sensor, position sensors)
- Motor and conveyor ready/active status
- Current operating mode (Automatic / Manual / Stop)
- Tank counter and system state indicators
- Control buttons: ACTIVATE, ROTATE, RESET
---
  
  ![Interface](Automatic%20Rotation%20System/docs/Interface.jpg)
---

##  Hardware Setup & Wiring

The physical test rig was built from scratch:

- Assembled electrical cabinet with proper cable management and labeling
- Wired 24VDC control circuit independently (PLC, sensors, relays, HMI)
- Three-phase motor wiring with shielded cable, properly grounded
- Frequency inverter configured for optimal motor parameters (frequency, voltage, ramp times)
- Color-coded wiring following company industrial standards
- All components labeled per electrical schematic conventions

---

##  Development Approach

The software was built **modularly** — each functional block was tested independently before integration:

1. Motor rotation + position sensor feedback
2. Conveyor start/stop logic
3. Fill detection with timer confirmation
4. Mode switching and interlocks
5. Emergency/reset state machine
6. HMI integration and simulation via PLCSIM

This approach ensured stable, predictable behavior and made debugging significantly easier on physical hardware.

---

##  Results

- Full system demonstration to company engineers and management
- Software verified as fully functional and meeting client specifications
- Physical test rig validated the complete process logic
- Project used as the basis for the final client-delivered system

---

##  Technologies & Tools
- **Siemens TIA Portal V18** — PLC programming (LAD/FBD), HMI design
- **PLCSIM** — offline simulation and testing
- **Siemens S7-1200** — PLC hardware
- **KTP400 Basic** — HMI panel
- **PROFINET** — industrial Ethernet communication
- **IE5338** — inductive position sensors
- **OJ5126** — photoelectric barrier sensors
- **Frequency inverter** — motor speed and torque control

---

##  Author

**Duma Octavian Ioan**  
Student — Automation and Applied Informatics  
Faculty of Automation and Computer Science, Technical University of Cluj-Napoca

Internship project at **Neotronik Automation** | Jul–Sep 2025 

---

## License

This project was developed during an industrial internship. All rights regarding the commercial deployment belong to Neotronik Automation. The code and documentation are shared here for portfolio and educational purposes.
