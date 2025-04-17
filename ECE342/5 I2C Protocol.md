---
tags:
  - ECE342
creation date: 2025-02-18 14:51
modified date: 2025-04-17 08:41
---

# Inter-Integrated Circuit (I2C)
- 2 wire ***serial*** protocol 
	- Serial **Clock** Line (SCL)
	- Serial **Data** Line (SDA)

- *only master can initiate communication* (since master is responsible for toggling SCL and thus needs special hardware)
- handshake-side 

- *open drain* config using *weak pull-up* resistors 
	- *open drain* - type of output config; output uses a single N-channel MOSFET/NPN bipolar transistor
		- output only low/float; requires pull-up resistor to define high state 
		- **pull the line to the ground (low state)** transistor inside device is turned on, connects output pin to GND, actively drives line to low state
		- **leave the line floating (high impedance/floating state)** transistor is turned off, disconnecting output pin from GND, leaving it in Z state (not actively connected to anything)
	- *weak pull-up resistors* - open drain cannot actively drive the line high; external pull-up to pull line to high state (VCC) when transistor is off
		- pulls line to a defined high state (weak 1) when no active device is driving it low
		- ensuring defined high state and low power consumption
		- **allows sharing of the line**
- unused bus reads as weak 1 

- when dev wants to use bus, it pulls the line **low**
	- when another dev sees the line is low, it will not transmit 
- **no dev can hold the bus high**; otherwise no one can use it 
	- *all devices can always listen/receive when any device pulls the line low*
- only controllers create the Clk 
	- dev can override this (clock stretching)
- supports different speed modes 
	- faster speed -> complex hardware 
	- standard 100KHz; high speed 3400 KHz 

- **unique ID** for each dev (7 bits)
	- Controller sets the ID of the dev it wants to communicate with (all device RX)
	- dev only uses bus if ID matches dev's own ID 
- I2C dev can both transmit and receive or both(TX & RX)


## I2C Transmission 
- Each byte is sent **one bit** a time *MSB first*
	- synchronized to each pulse of SCL 
	- **SDA only changes when SCL is low** (ensure SDA is stable on both edges of SCL)

![[Pasted image 20250207121143.png|600]]

## I2C Start & Stop Bits 
- when bus is idle, SCL & SDA float high (pull-up resistors)
	- to **start** using bus, SDA is pulled low while SCL is high
	- SCL toggles while SDA carries data 
	- upon completion, SDA is set high to **stop** ***while SCL is high***, SCL go back to float high 
		- otherwise if SDA changes high while SCL is low, becomes indistinguishable from normal transmission 

![[Pasted image 20250207121650.png | 500]]


## I2C ACK & NACK bits 
- To tell data was received correctly...
- After each byte, TX and RX swap roles for 1 bit 
	- RX sends 1 bit to TX
		- 0 = byte receive (ACK)
		- 1 = byte NOT received (NACK)
	- ACK/NACK uses the SDA line 

![[Pasted image 20250207122037.png|500]]

## I2C Register Write
![[Pasted image 20250207122256.png | 600]]

## I2C Register Read 
![[Pasted image 20250207122402.png|600]]


## NACK
![[Pasted image 20250207122457.png | 600]]

## I2C Burst Transfer 
- can send as many bytes in a single transmission (1 START 1 STOP)
	- can mix reading and writing multiple locations in a single transmission with chaining
	- but no one else can use the bus...

## I2C Clock Stretching 
- possible but not typical
- how 
	- controller pulls SCL low (controller controls Clk frequency)
	- when controller releases SCL, if device want to stretch, device will keep/pull SCL low
	- moment where device catches and holds the clock 
	- controller keeps trying to release SCL (let it go high) but device is holding it low 
![[Pasted image 20250211115847.png|500]]


## I2C Arbitration 
- when multiple controllers want to use this bus...
	- controllers constantly monitor SCL and SDA to check the bus is free before transmitting - if busy, wait for stop bit before transmitting 
- when 2 controllers pull SDA low at the **exact same time**
	- each controller pulls SDA low and checks to see if it went low

> I2C requires that if a master in a multi-master environment transmits a high, but see's that the line is low (another device is pulling it down), to halt communications because another device is using the bus.

- if low, that controller has bus control 
	- otherwise, controller switches to device mode until it sees a STOP bit, and will try to get control again

- (If both happens to transmit the same bit) The arbitration continues bit by bit until one master notices a discrepancy and backs off. The device sending the lowest address value will win because a 0 bit (low) overrides a 1 bit (high) on the **wired-AND bus**

## Derivative Protocols 
- System Management Bus (SMBus)
	- used on computers for power related chips i.e. battery 
- Improved Inter-Integrated Circuit (I3C)
	- backward compatible with I2C
	- speed 12.5MHz 
	- supports **dynamic address allocation**

## TI Manual
https://www.ti.com/lit/an/slva704/slva704.pdf
