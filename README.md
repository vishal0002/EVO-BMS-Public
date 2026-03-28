# EVO BMS — Dual-Pack Energy Architecture

> *A system is not safe because it works — it is safe because it behaves correctly when it fails.*

---

🌐 **[View the Interactive Architecture Teaser](https://bms.evovhil.com)**
*(A visual walkthrough of the system and failure behavior)*

---

## Overview

This project explores a safety-critical dual-pack Battery Management System (BMS) architecture designed for compact electric vehicles. The system fundamentally separates the **Energy domain** (84S HV traction) from the **Control domain** (4S LV survivability), enforcing a strict rule:

**Control must outlive power.**

---

## High-Level Architecture

```text
┌────────────────────────────┐
│        84S HV Pack         │
│  (Master / Slave BMS)      │
└────────────┬───────────────┘
             │
     Energy Management
             │
┌────────────▼───────────────┐
│   Controlled Extraction    │
│   (Balancing Mechanism)    │
└────────────┬───────────────┘
             │
       Energy Routing
             │
┌────────────▼───────────────┐
│        12V LV Rail         │
│  (Independent LiFePO4)     │
└────────────┬───────────────┘
             │
  ┌──────────▼──────────┐
  │   Control Systems   │
  │  (VCU / BMS / ESC)  │
  └──────────┬──────────┘
             │
       Torque Control
             │
┌────────────▼───────────────┐
│        Power Stage         │
│        (ESC Brawn)         │
└────────────────────────────┘
```

---

## Design Intent

The system must remain controllable under all failure conditions.
If control is lost, energy must be removed.

---

## Core Design Principles

**Control Survivability**
Loss of traction power does not imply loss of control. The LV layer remains active long enough to enforce a safe state.

**Energy-Aware Behavior**
The system continuously evaluates energy availability, load demand, and stability to dynamically adapt behavior.

**Graceful Degradation**
The system rejects binary failure in favor of a staged response:

```
Recover → Warn → Degrade → Shutdown → Safe State
```

**Brain vs. Brawn Separation**
The LV-powered *Brain* handles decisions, while the HV-powered *Brawn* executes energy delivery. No brain equals no torque.

---

## Safety Philosophy

A safe system is defined by the absolute prevention of uncontrolled energy delivery.

In this architecture:
- Control loss leads directly to **torque inhibition**
- System failure results in immediate **energy isolation**

> The system is not designed to avoid failure — it is designed to **fail correctly**.

---

## Software-Defined Validation

This system is developed and validated using a full virtual Hardware-in-the-Loop **([EVO vHIL](https://evovhil.com))** environment.

- Embedded BMS firmware runs on simulated hardware
- Control logic executes in a real-time RTOS environment
- Fault conditions are injected deterministically via software

This allows safety behavior to be validated **before physical hardware is deployed**.

The architecture has been exercised under simulated fault conditions, including:

- Communication loss
- Sensor anomalies
- Thermal events
- Watchdog-triggered failures

These scenarios are used to validate **deterministic system response** and **safe-state transitions**.

---

## Current Status & Roadmap

This repository focuses on system architecture, safety behavior, and design philosophy.

Implementation details, firmware, and validation pipelines are currently under active development and will be progressively integrated.

The goal is to evolve this into a fully verifiable, **software-defined validation platform** for high-voltage EV powertrains.

---

## License

MIT License — Copyright © 2026 Vishal Bagade
