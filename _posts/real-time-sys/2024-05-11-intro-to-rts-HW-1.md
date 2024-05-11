---
layout: post
title: real time systems - Intro I (HW)
author: Venkata Naga Ravikiran Bulusu
categories: Real time system series
---

Hello **EMBEDIN** viewers,

- Real-time systems require hardware components that can meet stringent timing constraints and ensure predictable behavior.
- Key requirements:

  1\. **Clock and Timer**:

    - Real-time systems rely on accurate clocks and timers to schedule tasks and events.
    - Components need precise clock sources with low jitter to ensure accurate timing.
    - examples:
    
      - SysTick timers is the heart beat of RTOS as discussed in previous post for task switching.
      - [RTC](https://www.allaboutcircuits.com/technical-articles/introduction-to-microcontroller-timers-periodic-timers/)


  2.\ **Processor**:

    - The processor should have sufficient processing power to execute tasks within their deadlines.
    - Real-time systems often require deterministic execution, so processors with predictable instruction execution times and minimal<br>
      interrupt latency are preferred.
    - examples:

      - non-real time capabilities: ARMv8A (aarch64)
      - real-time capabilities: ARMv7-M (arm32) and ARMv7-R (arm32)
      - [Overview](https://medium.com/@wassimdhokkar/get-first-introduction-to-different-arm-processors-39679593c0d6)
      - [ARMv7-R/R5](https://developer.arm.com/Processors/Cortex-R5)


  3\. **Memory**:

    - Both volatile (RAM) and non-volatile (ROM, Flash) memory should have predictable access times to support real-time operations.
    - Memory access latency should be minimized to ensure timely data retrieval and storage.
    - examples: Flash, SRAM, DRAM


  4\. **I/O Interfaces**:

    - Hardware interfaces for input and output should have low latency and deterministic behavior.
    - This includes interfaces for communication with sensors, actuators, networks, and other peripherals.
    - examples: I2C, SPI, CAN, UART


  5\. **Interrupt Handling**:

    - Interrupt latency can significantly impact real-time system performance.
    - Hardware components should support fast and prioritized interrupt handling to minimize latency and ensure timely response to external events.
    - examples: emergency braking systems


  6\. **Bus Architecture**:

    - The system bus architecture should minimize contention and provide efficient data transfer between components.
    - Dedicated buses or high-speed interconnects can reduce communication latency and improve overall system performance.
    - examples:

      - [AMBA](https://iamradhakulkarni.blogspot.com/2023/06/understanding-amba-protocol-apb-ahb-and.html)
      - [ARM AMBA](https://developer.arm.com/documentation/102202/0300/What-is-AMBA--and-why-use-it-)


  7\. **Power Management**:

    - Power management mechanisms should be designed to minimize energy consumption without compromising real-time performance.
    - Low-power states should be carefully managed to ensure timely wake-up and recovery.
    - examples: spacecraft to save power, generated from solar energy and batteries


  8\. **Fault Tolerance**:

    - Real-time systems often operate in mission-critical environments where system failures can have severe consequences.
    - Hardware components should incorporate fault-tolerant features such as redundancy, error detection, and error recovery mechanisms.
    - examples:

      - CRC checks on medical systems to handle patients data
      - Implement a redundant memory array with error correction codes and dynamic redundancy management for fault tolerance in space systems,<br>
        ensuring reliability through robust voting logic and environmental hardening.


  9\. **Temperature and Environmental Considerations**:

    - Components should be designed to operate reliably across a range of environmental conditions, including temperature, humidity, and vibration.
    - Thermal management solutions may be necessary to prevent overheating and ensure consistent performance.
    - examples: satellite systems to protect the hardware components and payload


  10\. **Testing and Verification**:

    - Hardware components should undergo rigorous testing and verification to ensure they meet real-time requirements under various operating<br>
      conditions.
    - This includes testing for timing accuracy, reliability, and compliance with relevant standards.
    - examples: medical systems

Overall, hardware components in real-time systems must be carefully selected and configured to meet stringent timing constraints, ensure deterministic behavior, and provide reliable operation in mission-critical applications. Next time, we shall start with a design problem on timers, stay tuned!

<!-- begin wwww.htmlcommentbox.com -->
 <div id="HCB_comment_box"><a href="http://www.htmlcommentbox.com">Widget</a> is loading comments...</div>
 <link rel="stylesheet" type="text/css" href="https://www.htmlcommentbox.com/static/skins/bootstrap/twitter-bootstrap.css?v=0" />
 <script type="text/javascript" id="hcb"> /*<!--*/ if(!window.hcb_user){hcb_user={};} (function(){var s=document.createElement("script"), l=hcb_user.PAGE || (""+window.location).replace(/'/g,"%27"), h="https://www.htmlcommentbox.com";s.setAttribute("type","text/javascript");s.setAttribute("src", h+"/jread?page="+encodeURIComponent(l).replace("+","%2B")+"&mod=%241%24wq1rdBcg%24RmXC2fLP9uwV4kXjhF9Do."+"&opts=16798&num=10&ts=1715334874839");if (typeof s!="undefined") document.getElementsByTagName("head")[0].appendChild(s);})(); /*-->*/ </script>
<!-- end www.htmlcommentbox.com -->
