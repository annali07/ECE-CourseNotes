---
tags:
  - ECE342
creation date: 2025-04-16 10:16
modified date: 2025-04-16 11:21
description: 
---
- time critical embedded systems 
- pacemakers
	- micro-second level -> deliver electric impulses to heard
- fight control systems 
- power systems 

# Real Time OS
- RT -> meeting strict deadlines 
- NOT fast computing 
	- precise timing and high degree of reliability 
	- handle interrupts and sys exceptions timely 
	- ensure critical sections of code are done within allocated time 

# Hard V.S. Soft RT Deadline 
- hard -> air bags, fly-by-wire 
- soft -> video streaming, home temperature monitor 

![[file-20250416105329889.png]]

# Comparison with GP-OS
- GP-OS focuses on high throughput (many applications running at the same time)
	- RR, runs when task finishes 
- RT-OS -> predictable latency (each task has a strict deadline)
	- priority based pre-emptive scheduling where higher priority tasks take precedence 
	- runs at a fixed time interval, set with a hardware timer 
- **scheduler difference**

![[file-20250416105937296.png]]

![[file-20250416110241152.png]]

# Context Switching
![[file-20250416110426447.png]]

![[file-20250416110439749.png]]

# Bare Metal V.S. RTOS
- bare metal
	- super-loop approach, every task is run one after the other 
	- each task can take different amounts of time to finish, no way to guarantee a fixed latency for any of them 
- RT-OS 
	- create **tasks** and hand over duties to OS scheduler 
	- task priorities and task deadlines are specified at time of task creation
	- programmer must make sure the hardware is capable of completing all the required tasks in the specified time 


