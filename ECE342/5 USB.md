---
tags:
  - ECE342
creation date: 2025-04-03 12:41
modified date: 2025-04-14 18:22
description: 
---

- Before USB, computers used serial ports 
	- can still connect over a COM port 
- Plug & Play protocol 

## USB A 2.0 && USB A 3.0
![[file-20250403124325405.png|500]]

| Property               | USB-A 2.0                                       | USB-A 3.0                 |
| ---------------------- | ----------------------------------------------- | ------------------------- |
| Speed                  | 480Mbps                                         | 5 Gbps                    |
| Physical               | black/white                                     | blue                      |
| Power                  | 500mA (2.5W)                                    | 900mA (4.5W)              |
| Backward Compatibility | USB-A 3.0 ports accept 2.0<br>but at 2.0 speeds | accepts 3.0, at 2.0 speed |

# USB Workings 
- **host** to **device** protocol 
- host controls all communication 
	- USB 1.1 - only host OR a device can be communicating at a time (half duplex)
		- one dev at a time can transmit to host 
	- USB 2.0 - full duplex 

![[file-20250403124830147.png|500]]

##  USB 1.1
![[file-20250403125032447.png]]


# Connecting USB to Device
![[file-20250403125133855.png]]


# USB - Physical Layer 
![[file-20250403125552416.png]]
![[file-20250403130005544.png]]


# USB Packets

![[file-20250403130017448.png]]


# USB - Transfer Types
![[file-20250403130041969.png]]


# CRC (Cyclic Redundancy Check)
- Robust form of error checking
	- USB uses CRC to ensure data received correctly 
- CRC is specified using the number of bits used.  
	- More CRC bits means more errors can be detected.  
	- USB uses CRC5 and CRC16
- Parity bit in UART is CRC1


# USB 2.0 + 
- USB 2.0 adds high-speed 
	- higher speed configured via packets 
- USB 3.0 uses different cable 
	- adds 2 more pairs of signals 
	- supports full duplex 
	- switches to 8b/10b encoding 

![[file-20250403130308044.png|500]]


# 8b/10b Encoding 

![[file-20250403130341387.png]]








