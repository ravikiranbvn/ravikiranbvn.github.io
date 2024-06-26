---
layout: post
title: real time systems - Intro I (SW)
author: Venkata Naga Ravikiran Bulusu
categories: Real time system series
---

Hello **EMBEDIN** viewers,

- A **real-time system** is characterized by its ability to produce the expected result within a time (to meet deadlines) and time synchronization<br>
  between different tasks
- classification:

  - **Hard real-time systems**: missing deadlines, can cause system failure, human loss

    - examples: Mars rover, medical devices and mission-critical systems

  - **Soft real-time systems**: missing deadlines, can cause low quality output, with small or insignificant consequences

    - examples: personal computers, audio-systems not responding on time

Features:

- reliability, safety, maintainability, availability and security
- Time constraints:

  - event response (interrupt latency)
  - scheduling (jitter between task switching)

- [Intro-to-real-time-systems](https://www.allaboutcircuits.com/technical-articles/introduction-to-real-time-embedded-systems/)

- Therefore, we need a design and implementation to monitor and maintain such kind of a system

- Real Time Operating Systems (RTOS) provide such solutions:

  - design patterns for scheduling (non-preemptive and preemptive) and queueing
  - smaller foot print of kernel
  - effective CPU utilization
  - task priority, multi-threading, synchronization, Inter processor communications (IPC) mechanisms, interrupt handling
  - file system handling, debugging, tracking/monitoring of tasks
  - SysTick timer along with scheduler, handles the task switching

- **RTOS examples: FreeRTOS, QNX**

In essence, achieving real-time systems necessitates a meticulous customization of both hardware and software components. Next time, we will see more about real-time systems from hardware point of view. stay tuned! 

<!-- begin wwww.htmlcommentbox.com -->
 <div id="HCB_comment_box"><a href="http://www.htmlcommentbox.com">Widget</a> is loading comments...</div>
 <link rel="stylesheet" type="text/css" href="https://www.htmlcommentbox.com/static/skins/bootstrap/twitter-bootstrap.css?v=0" />
 <script type="text/javascript" id="hcb"> /*<!--*/ if(!window.hcb_user){hcb_user={};} (function(){var s=document.createElement("script"), l=hcb_user.PAGE || (""+window.location).replace(/'/g,"%27"), h="https://www.htmlcommentbox.com";s.setAttribute("type","text/javascript");s.setAttribute("src", h+"/jread?page="+encodeURIComponent(l).replace("+","%2B")+"&mod=%241%24wq1rdBcg%24RmXC2fLP9uwV4kXjhF9Do."+"&opts=16798&num=10&ts=1715334874839");if (typeof s!="undefined") document.getElementsByTagName("head")[0].appendChild(s);})(); /*-->*/ </script>
<!-- end www.htmlcommentbox.com -->
