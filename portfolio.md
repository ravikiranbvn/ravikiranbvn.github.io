---
layout: page
title: Portfolio
permalink: /portfolio/
---

## Medical Systems Case Study  
### Heart-Lung Machine Control Unit — B1 Engineering Solutions

Heart-lung machines are **safety-critical medical devices** used during open-heart surgery to temporarily replace the function of the heart and lungs. These systems must operate **continuously, deterministically, and safely**, as any malfunction can have life-threatening consequences. The software development followed **IEC 62304** guidelines, with strict separation of safety classes and clearly defined responsibilities across system components.

### System Overview

The overall system consisted of **two primary computing domains**:

- **Class C Device (Safety-Critical Control):**  
  An **STM32F7-based main controller unit**, responsible for real-time pump control, sensor acquisition, and machine control logic. This unit executed all safety-critical algorithms and operated under strict real-time and determinism constraints.

- **Class A Device (User Interface):**  
  A GUI control system based on an **i.MX8 SoC running Embedded Linux**, responsible for visualization, configuration, and operator interaction. This system was explicitly kept outside the safety-critical control path.

### Safety Isolation & Communication Architecture

Direct communication between the **Class A GUI system** and the **Class C main controller** was **not permitted**.  
All interaction was routed through a dedicated **safety processor (Class A)**, which acted as a validated communication gateway.

Key characteristics of this design:

- The safety processor implemented a **minimal, validated clone of critical STM32F7 interfaces**
- Communication occurred over **two physically separate CAN buses**
- Strict isolation ensured that GUI failures or malformed inputs could **never directly influence safety-critical control logic**
- All messages were validated, filtered, and arbitrated before reaching the main controller
- The architecture supported fault containment, validation, and traceability required for certification

### Team & Development Context

The project was developed by a **distributed team of ~20 engineers**:

- **GUI team:** Canada  
- **Main controller & safety processor teams:** Munich, Germany  

I was part of the **main controller software team** in Munich, working under system architects and a team lead, and was responsible for developing and maintaining multiple core software components.

### My Responsibilities

My work focused on **safety-critical embedded software development** on the STM32F7 platform, including:

- Pump configuration logic and **device number allocation**
- **CAN arbitration algorithms** for multi-device coordination
- **Non-volatile memory (NVM) file system**, including multiple handlers for:
  - Configuration parameters
  - Logging data
  - Raw operational data
- Design, refactoring, and maintenance of **Hardware Abstraction Layers (HAL)**
- Use of **C/C++ design patterns** suitable for safety-critical systems
- **Multi-threaded real-time implementation** using **Keil RTX5 RTOS**

### Software Design Principles

The software followed strict engineering and certification-driven rules:

- **No dynamic heap allocation**
- Clear separation of **hardware-dependent vs hardware-independent code**
- Deterministic execution and bounded timing behavior
- Layered architecture with clear ownership boundaries

At a high level, the system architecture followed:

- **Service-Oriented Architecture (SOA)**
- **Publish–subscribe communication model**
- **Object-oriented design (OOD)**
- Strict **layered software design**

### Documentation & Compliance

The project emphasized strong documentation and traceability:

- Detailed **architecture and design documentation** using Enterprise Architect
- Doxygen-based API documentation integrated with formal Word documentation
- Alignment with IEC 62304 development and verification processes

---

> Note: Detailed architectural diagrams and implementation artifacts are omitted due to confidentiality and regulatory constraints.
