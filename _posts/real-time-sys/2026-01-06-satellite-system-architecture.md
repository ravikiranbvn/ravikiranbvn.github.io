---
layout: post
title: "Satellite Systems: Architecture, Hardware, Software, and FDIR"
subtitle: "Onboard and payload computers, redundancy, and fault management in satellites"
date: 2026-01-06
categories: [embedded, space, systems-architecture]
tags: [satellite, space-systems, obc, payload, redundancy, fdir]
---

## Satellite Systems: Architecture, Hardware, Software, and FDIR

Satellite systems are among the clearest examples of **high-reliability embedded systems**.  
Once launched, there is no physical access, no repair, and often no second chance. This reality forces satellite designers to prioritize **architecture, isolation, redundancy, and fault management** long before performance or features.

This article uses **satellites as a concrete space-system example** to explain how onboard and payload computers are structured, how hardware and software responsibilities are separated, and why **FDIR (Fault Detection, Isolation, and Recovery)** is central to survival.

---

## 1. Satellite System Architecture

### 1.1 Why Satellites Are a Good Example

Satellite systems make architectural decisions explicit:

- They operate remotely for years
- Physical repair after launch is impossible
- Failures must be handled autonomously

At a system level, satellites are part of a broader architecture consisting of:
- the **space segment** (the satellite),
- the **ground segment**, and
- the **control segment**.

A good high-level overview of this system-level view is:
- https://www.techtarget.com/searchnetworking/tip/An-introduction-to-satellite-network-architecture

---

### 1.2 The Core Architectural Split

Most satellites follow a strict separation of responsibilities:

| System | Responsibility |
|------|----------------|
| **Onboard Computer (OBC)** | Satellite survival and platform management |
| **Payload Computer** | Mission-specific data processing |

This separation is the foundation of fault containment and safe operation.

---

### 1.3 Onboard Computer (OBC)

The **Onboard Computer** is the **safety-critical controller** of the satellite.

**Typical responsibilities**
- Attitude determination and control (ADCS)
- Orbit and maneuver control
- Power and thermal management
- Telemetry, tracking, and command (TT&C)
- Fault Detection, Isolation, and Recovery (FDIR)

**Architectural characteristics**
- Hard real-time and deterministic behavior
- Minimal dynamic complexity
- Clearly defined operating modes (normal, degraded, safe mode)
- Often duplicated (primary + backup)

> **Rule of thumb:**  
> If the OBC fails, the satellite is lost.

The OBC defines the satellite’s safe state and has ultimate authority during anomalies.

---

### 1.4 Payload Computer

The **payload computer** exists to generate **mission value**.

**Typical payload tasks**
- Earth-observation / Space-observation image processing
- Scientific data handling
- RF / communications signal processing
- Compression and encryption
- Increasingly, onboard AI/ML inference

**Architectural characteristics**
- Performance-driven and parallel
- High data throughput
- Restartable and recoverable (within limits)
- Strictly isolated from satellite survival logic

> Payload failure may lose data.  
> OBC failure loses the mission.

---

### 1.5 Safety Controllers Inside Payloads

Complex payloads often include a **local supervisory controller** (hardware or tightly constrained software).

Its purpose is to:
- Monitor payload health
- Enforce safe startup and shutdown
- Perform local fault containment
- Prevent payload failures from propagating into the platform

This results in **layered safety**:
- Satellite-level safety → OBC
- Payload-level safety → local supervisor

---

### 1.6 A Non-Negotiable Architectural Rule

> **Payload software must never directly control satellite survival functions.**

All OBC ↔ payload interactions are:
- Command-based
- Validated
- Rate-limited
- Designed to fail safely

This boundary is one of the most important protections against mission-ending failures.

---

## 2. Hardware Architecture (HW)

### 2.1 Onboard Computer Hardware

OBC hardware is chosen for **predictability and fault tolerance**, not raw performance.

**Typical characteristics**
- Radiation-hardened or radiation-tolerant processors
- ECC memory and buses
- Redundant power rails and communication paths
- Conservative clocks and well-understood failure modes

Representative OBC platform families include:
- LEON-based rad-hard / rad-tolerant processors (e.g., GR712 / GR740 class)
- Rad-hard single-board computers (e.g., RAD750-class designs)

---

### 2.2 Rad-Hard vs Rad-Tolerant Components

Radiation is a primary environmental threat in space. Electronic components are commonly categorized as:

**Radiation-hardened (rad-hard)**
- Designed and qualified to survive high radiation levels
- Used for long-life and safety-critical functions
- Higher cost, limited availability

**Radiation-tolerant (rad-tolerant)**
- More resistant than standard commercial parts
- Often combined with redundancy and mitigation
- Suitable for shorter missions or less harsh environments

A practical explanation of these trade-offs:
- https://militaryembedded.com/comms/satellites/rad-hard-vs-rad-tolerant-a-guide-to-the-differences-and-applications-in-military-electronics

---

### 2.3 Payload Hardware

Payload hardware is selected for **throughput and data movement**:

- Multi-core SoCs
- CPU + FPGA combinations
- Hardware accelerators (DSP, GPU, AI)
- High-bandwidth memory and I/O

Payload hardware typically accepts more risk than OBC hardware, relying on isolation and recovery rather than absolute fault immunity.

---

## 3. Software Architecture (SW)

### 3.1 Onboard Computer Software

OBC software is intentionally conservative.

**Key characteristics**
- Deterministic scheduling
- Explicit state machines
- Limited dynamic allocation
- Strong fault handling and mode control
- Extensive verification and long-term maintenance mindset

**Languages**
- **C** dominates core flight software
- **C++** is used with strict restrictions in some systems
- **Rust** is still uncommon on safety-critical flight paths today

Correctness and analyzability outweigh convenience.

---

### 3.2 Payload Software

Payload software handles most system complexity:

- Large data pipelines
- Parallel processing
- High I/O and storage usage
- Restart and recovery semantics

**Languages**
- **C++** for performance-critical pipelines
- **C** for drivers and low-level interfaces
- **Rust** is increasingly attractive for safer concurrency and networking in payload systems

---

### 3.3 OBC–Payload Interface Design

The OBC treats the payload as a **managed subsystem**, not a peer.

Interfaces are:
- Command-driven
- State-based
- Validated on both ends
- Designed for graceful degradation

Payload software never enforces satellite safety on its own.

---

## 4. FDIR: Fault Detection, Isolation, and Recovery

FDIR is not a feature in satellite systems — **it is a survival mechanism**.

Because repair is impossible after launch, the satellite must autonomously:
1. Detect faults,
2. Isolate the faulty element,
3. Recover or transition to a safe state.

FDIR logic primarily resides in the **OBC**, tightly integrated with redundancy.

---

### 4.1 Fault Detection

Fault detection answers one question:  
*Is the system behaving outside expected bounds?*

Common mechanisms:
- Hardware and software watchdogs
- Missed heartbeats from subsystems
- Telemetry limit checks (voltage, current, temperature)
- Timing violations and execution overruns
- Sensor cross-checks and consistency tests

False positives are acceptable.  
False negatives are dangerous.

---

### 4.2 Fault Isolation

Once a fault is detected, the system must determine **where it occurred**.

Isolation techniques include:
- Power-cycling or resetting a specific subsystem
- Disconnecting a failed bus or sensor
- Masking faulty data sources
- Switching away from a suspected processor or path

Good isolation prevents **fault propagation**.

---

### 4.3 Fault Recovery

Recovery defines what the satellite does next.

Typical actions:
- Reset a payload processor
- Switch from OBC-A to OBC-B
- Reconfigure buses or power paths
- Enter safe mode
- Continue operation in a degraded configuration

Recovery is usually **hierarchical**:
- Payload-level recovery first
- Platform-level recovery if needed
- Safe mode as the last line of defense

> Safe mode is not failure — it is controlled survival.

A practical lesson from NASA on spacecraft fault management:
- https://llis.nasa.gov/lesson/839

---

## 5. Redundancy in Satellite Systems

Redundancy exists because space does not allow physical repair.

**Common redundancy patterns**
- Cold redundancy (powered-off backup)
- Warm redundancy (partially powered backup)
- Hot redundancy (parallel operation)
- Cross-strapped buses and power paths

**What typically gets duplicated**
- OBCs (A/B chains)
- Power regulation and critical rails
- Communication paths
- Reset and watchdog logic
- Sometimes sensors and storage

Redundancy provides options; **FDIR decides when and how to use them**.

---

## 6. Key Takeaway

Satellite systems are designed around **controlled failure**.

- The OBC keeps the satellite alive
- The payload generates mission value
- FDIR decides how the system survives faults
- Redundancy makes recovery possible

> In satellites, architecture defines not just how the system works —  
> but how it fails, and whether it survives that failure.

---

## Further Reading & References

- https://www.techtarget.com/searchnetworking/tip/An-introduction-to-satellite-network-architecture
- https://s3vi.ndc.nasa.gov/ssri-kb/static/resources/aerospace-07-00146-v2.pdf
- https://militaryembedded.com/comms/satellites/rad-hard-vs-rad-tolerant-a-guide-to-the-differences-and-applications-in-military-electronics
- https://llis.nasa.gov/lesson/839