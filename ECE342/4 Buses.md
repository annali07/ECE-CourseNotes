---
tags:
  - ECE342
creation date: 2025-02-16 12:08
modified date: 2025-04-14 09:56
---

- mechanism of communication between CPU, mem, dev 
	- wires and a protocol : clk, control, addr, data lines 
	- **throughput** (datawidth * bits) - overhead

#### Types
- serial - 1 bit conveyed at a time (UART, I2C, SPI, USB)
- parallel - N bits (Memory, PCI)
- on-chip - connecting cores within a dev (SoC/FPGA)
- off-chip - connecting modules on PCB
- synchronous - separate clk and data signals
- asynchronous - no clk signal (dev provides own clk)

#### Controller/Device Config
- bus operates using **controller/device** config 
	- controller handles requests, ACK
	- bus can have multiple controllers (needs arbitration on who controls the bus)

#### Bus Protocol
- bus protocol defines what controller/dev should do 
	- controller initiates data transfer 
	- dev responds to init request (processor/peripheral/mem)
- protocol defines 
	- **data direction** receive(RX) transmit (TX)
	- how data shares wires
	- addr indicates data src and dest

![[Pasted image 20250207110539.png|500]]

#### Bus Splitting
- to prevent bus from getting very large, share line between addr and data OR splitting data across multiple transfers 
![[Pasted image 20250207111055.png | 600]]

#### Bus Control Methods 
The controller knows that device is done via...
##### Strobe 
- controller asserts req
- dev puts data on bus within deadline 
- controller receives data and de-asserts req
- dev ready for next req 

**Pro/Con**
- when dev response time is known, can avoid the extra line for ACK (can set DDL)
- can complicate logic of controller, since it waits a fixed number of cycles internally

##### Handshake 
- controller asserts req 
- dev puts data on bus and ACK the data is ready. controller reads data 
- controller de-asserts req 
- dev stops transmitting data and de-asserts ACK (requires **extra line for ACK**)
- both ready for next req 
UART is handshake

**Pro/Con**
- extra line for ACK 
- may be slower than strobe - requires controller to detect ACK 


#### Burst Transfers 
Motivation: Reading a series of consecutive memory locations; avoid putting each on the line
- can read from a contiguous sequence of addr at once 
- start addr given by CPU; **bursts of data** are received from **n** successive locations 

**Pro/Con**
- uses resources efficiently (continuous stream without interrupts)
- sending consecutive blocks = **maximize throughput reduces latency**
- however, one dev can occupy bus for long period, blocking another dev's access to bus

![[Pasted image 20250207112110.png|600]]


## Bus Arbitration 
- when multiple dev simultaneously request the bus, need to decide who controls the bus
- handled by **arbiter** which receives all the req and grants req based on **arbitration scheme**

#### Priority-based Arbitration 
- each dev sends *separate req* to hardware arbiter, which schedules requests 
	- **fixed priority/round-robin**
		- RR - dev used bus most recently gets lowest priority
		- prevents overusing the bus but complicates arbiter design
![[Pasted image 20250207113243.png | 500]]

#### Daisy-Chain Arbitration 
- peripherals are *daisy-chained* and req go through multiple dev before getting to controller 
	- **upstream** (closer to controller) receive req from **downstream** and pass along 
	- devices are responsible for arbitration 
- controller sees only one req and issues a grant
	- peripheral closest to controller gets served first 
**Pro/Con**
- Pro - no need for dedicated hardware arbiter; easier to add more dev to the same bus 
- Con - works best only when highest priority peripheral is closest to processor 
	- difficult to add a dev with lower/higher priority in middle of chain 
![[Pasted image 20250207113632.png|500]]


## Bus - Memory Mapped I/O
- bus affects how memory mapping works 
	- mem-mapped peripherals are each assigned a range of addresses 
	- address decoder decides which component receives the data 
	- decoder asserts the write enable signal to appropriate dev 
![[Pasted image 20250207113916.png | 500]]

## Memory Map Address and Decoding 
- 512 MB of SRAM but only 128KB usable space to *simplify address decoder logic*

**Naive Scheme**
- a system with 3 peripherals 
- different enable signal for each device (the address accumulates)
	- for N devices, solve N truth tables to design hardware 

**Optimized Scheme**
- look at top few bits to know which device is being addressed 
- PRO - simpler address decoder (& directly use lower bits to address register inside dev)
- CON - wastes addresses 
