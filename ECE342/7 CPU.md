---
description:
  - "[[ECE342]]"
tags:
  - ECE342
modified date: 2025-04-15 17:01
---

# ISA
- ISA defines how the bits of the instruction are interpreted 
	- NUCLEO - Cortex-M4, ARMv7 ISA

#### Compiler Optimization
![[file-20250318110230288.png ]]

# SIMD 

**ARM-M4**
- z = \_\_SADD16(x, y) -> Macro to add 16|16 uint32_t and store in 16|16 in uint32_t
	- \_\_ASM - write assembly directly in C. Volatile - prevent compiler optimization
- instruction for FP addition 

- use libraries
	- CMSIS-DSP - signal filtering and FFT, Mel-frequency cepstrum(MFCC) speech processing
	- CMSIS-NN

# Embedded CPU
- factors 
	- bit-width - impacts total number of cycles 
	- hardware support - encryption accelerator/multiply/FP
	- fastest - throughput (BW) OR latency (clock rate)?
- user-time related to CPU clock (cycles)
$$Execution Time = Clock Cycles \times Clock Period$$
- bit-width affects total number of cycles 

# Cycles per Instruction (CPI)
$$Total Clock Cycles = \sum_{i \in I} |i| \times cycles per Instruction$$

# Factors affecting CPI 
- depends on architecture - w/wo hardware accelerator (hardware multiplier)
- \# of instructions depends on 
	- hardware 
		- ISA
		- architecture 
	- software
		- algorithm - more efficient 
		- programming language 
		- compiler 


- A program can have more instruction, but if lower CPI, execution time is smaller
![[file-20250318113355981.png]]


# CPU Performance
$$Execution\ Time = (\sum_{i \in I} \# \ of \ Instrctions \times cycles \ per \ instruction) \times \ clock \ period$$
![[file-20250318114703619.png]]

# IPS (Instructions Per Second)

![[file-20250318114834640.png]]
- MIPS (Million IPS) (10^6)

# FP Hardware Example
![[file-20250318115408129.png]]


![[file-20250318115453217.png]]

1e^-8 = 10ns
1e^-9 = 1ns
1e^-6 = 1us
1e^-3 = 1ms

![[file-20250318120041498.png]]
