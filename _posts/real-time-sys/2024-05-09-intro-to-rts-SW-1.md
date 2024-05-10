---
layout: post
title: real time systems - Intro I (SW)
categories: Real time system series
---

Hello EMBEDIN viewers,

- A real-time system is characterized by its ability to produce the expected result within a time (to meet deadlines) and time synchronization   
  between different tasks
- classification:
  - Hard real-time systems: missing deadlines, can cause system failure, human loss 
    - examples: Mars rover, medical devices and mission-critical systems
  - Soft real-time systems: missing deadlines, can cause low quality output, with small or insignificant consequences
    - examples: personal computers, audio-systems not responding on time

Features:
- reliability, safety, maintainability, availability and security
- Time constraints:
  - event response (interrupt latency) 
  - scheduling (jitter between task switching)
- [Into-to-real-time-systems](https://www.allaboutcircuits.com/technical-articles/introduction-to-real-time-embedded-systems/)
- Therefore, we need a design and implementation to monitor and maintain such kind of a systems
- Real Time Operating Systems (RTOS) provide such solutions:
   - design patterns for scheduling (non-preemptive and preemptive) and queueing
   - smaller foot print of kernel
   - effective CPU utilization
   - task priority, multi-threading, synchronization, Inter processor communications (IPC) mechanisms, interrupt handling
   - file system handling, debugging, tracking/monitoring of tasks
   - SysTick timer along with scheduler, handles the task switching
- RTOS examples: FreeRTOS, QNX

In essence, achieving real-time systems necessitates a meticulous customization of both hardware and software components.
Next time, we will see more about real-time systems from hardware point of view. stay tuned!