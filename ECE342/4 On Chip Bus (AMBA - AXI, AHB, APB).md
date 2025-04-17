---
tags:
  - ECE342
creation date: 2025-04-03 13:15
modified date: 2025-04-14 10:28
description: 
---

# AMBA (Advanced Microcontroller Bus Arch)

- Consists of 3 bus protocols 
	- **AXI** (Advanced eXtensible Interface)
		- Connects high-speed components such as OoO CPUs and DRAM memory 
	- **Advanced High-performance Bus** (AHB)
		- Connects lower speed *(in-order)* CPUs and SRAM memory 
	- **APB** (Advanced Peripheral Bus)
		- Connects slow peripherals such as UART, Timer, GPIOs with microcontroller 


## AXI
- AXI separates different tasks into 5 channels 
	- Each task can happen independently. Burst wrtes doesn't affect slow reads. 
	- Simple bus support only 1/2 channels 
- AXI supports multiple **outstanding** transactions 
	- A controller can have multiple requests pending at the same time 
		- multiple addresses being read from memory 

![[file-20250403132002786.png]]


# AMBA Channels 
![[file-20250403132021018.png]]

![[file-20250403132030774.png]]


![[file-20250414101352375.png]]


# Multiple Buses
- Systems have different buses operating at **different speeds**, connecting different modules 
- Need a bus interface/bridge to move data between them 

- High speed dev connected to high performance bus; low speed dev to different bus 
![[file-20250403132139082.png|500]]


# ARM Cortex M4
![[file-20250403132153808.png]]

# STM32 - F4x Devices
2 * AHB; 2 * APB buses

![[file-20250403134136534.png]]



# Why Multiple Buses
- More efficient use of the bigger bus 
	- pack operations to and from multiple slow dev over a single operation of the large bus 
- Simpler protocol for peripherals
	- Connecting UART or timer over AXI adds a lot of complexity for no benefit 
- Simpler address decoding logic 
	- Address decoder for the AHB bus can treat all APB peripherals as a single dev 


# Bus Interface (Bridge)
![[file-20250403134338105.png]]

![[file-20250403134600295.png]]

![[file-20250403134609336.png]]

