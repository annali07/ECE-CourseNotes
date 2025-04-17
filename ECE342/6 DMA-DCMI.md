---
tags:
  - "#ECE342"
description:
  - "[[ECE342]]"
modified date: 2025-04-15 09:49
---

# DMA (Direct Memory Access)
- hardware module 
	- data source/destination include Memory (RAM/ROM) or Memory-mapped I/O (peripherals)
- improves energy efficiency of system 
	- DMA simpler than CPU, less gates 
	- fewer gates = less power 
- on STM32, DMA connected to AHB bus and supports **2 modes**
	- *Normal* copy from start address to end address and stop when done 
	- *Circular* continuously copy from start to end address in a loop 

![[file-20250304151800993.png | 500]]

## DMA Triggers
- DMA performs a transfer based on a *trigger*
	- when peripheral -> memory, trigger could be new input 
		- ADC has new reading
		- DMA waits for the new reading to trigger before copying to memory
	- when copying from *memory to memory*, CPU = trigger 
		- copy image N+1 when CPU is working on image N 
		- CPU triggers DMA to copy the next image to RAM **USE IN PROJECT**
		- calls a **software trigger** 

## DMA Channels 
- **DMA controllers** offer multiple channels 
	- each channel 
		- transfers data independently 
		- has a source & destination 
		- amount of data to be transferred
		- data-width of transfers (how many bits are moved each time)
- one trigger is available per channel 
- when using multiple channels, channel 1 is highest priority 

![[file-20250304152214504.png | 500]]

## DMA Interrupts 
- DMA controllers supports interrupts to update the CPU 
- transfer interrupts when 
	- *transfer complete* fires when total amount of data has been transferred
	- *transfer error* fires when an error occurs during the transfer 
	- *half transfer* fires when half of the data has been transferred 
		- useful when the CPU can start working on partial data while DMA copies the rest 

## DMA Transfer Types
- Fly-by DMA 
	- data goes directly form source to destination 
	- supported on STM32 CPU M4 
- flow-through 
	- uses register in the controller 
		- data is read one at a time from source to register 
		- data is written from register into destination 
	- used when 
		- devices have register of different sizes 
		- when memory to memory transfer cannot be read/written in one cycle 

![[file-20250304152510040.png | 500]]

## DMA Configuration
- *which DMA controller* should be used? 
- *which channel* should be used? 
	- different channels have access to different resources (ADC, USART, I2C etc.)
- *which trigger* should be selected for that channel? 

**Priorities**
- hardware priority is higher; channel 1 highest 
- If using software priority, highest should be channel demanding highest BW 

# DCMI (Digital Camera Interface)
- OV7670 supports image sizes 
	- VGA: 640 x 480
	- CIF: 352 x 288
	- Custom down to 40x30
- Uses I2C compatible protocol (SCCB)
- 30fps 
- use DMA to copy the image to memory without CPU 

## Images
- each image frame is rows, row major order 
	- 8 bit greyscale image - 0 is black, 0xFF is white 

![[file-20250304152923326.png | 400]]

## RGB
- color-space
- OV7670 supports RGB444, RGB555 and RGB565
	- 444 is 4 bits red, 4 bits green, 4 bits blue

## YCbCr
- Y = avg of R, G, B
- Cb = blue difference, Cr = green difference 
- Just the Y channel gives greyscale image 
- OV7670 uses 8 bits for Y, Cb, Cr 
	- to save BW, skips Cb and Cr for odd-numbered pixels 
	- If Y_i, Cb_i and Cr_i are the components for pixel i, the bytes sent from the camera are 

![[file-20250304153156774.png | 500]]


## Timing

![[file-20250304153243859.png | 600]]

- VSYNC - pulse to pulse; indicates 1 whole frame 
- HREF - high when outputting each row
- $D[7:0]$ outputs each pixel of the row, byte by byte
- each pixel output is aligned to PCLK 

## OV7670 - I/O
- SCCB (Serial Camera Control Bus) Interface - based on I2C
- configure camera by writing to registers using I2C writes 
- OV7670 has no crystal oscillator, so cannot produce a clock 
	- need to provide clock (XCLK) for the camera, camera creates PCLK from XCLK (need it for fps)
![[file-20250304153506368.png | 300]]

## DCMI
- STM proviudes DCMI module which handles all the signalling and timing 
- connect DCMI to DMA, CPU isn't involved in transfer 


