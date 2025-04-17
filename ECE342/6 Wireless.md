---
tags:
  - "#ECE342"
description:
  - "[[ECE342]]"
modified date: 2025-04-15 10:15
---

# Wireless communications
- wireless transmissions modulate a high frequency sinusoid (carrier signal) based on the message transmitted 
- allows for **frequency division multiplexing (FDM)**
	- multiple msg can be transmitted at the same time at different frequencies 
- *analog* carrier signal is modulated by a discrete signal 
	- **modulation uses DAC, demod uses ADC**

## Protocols
- Wi-Fi, Bluetooth/BLE, NFC, Zigbee, LoRaWAN, Z-Wave...
	- short vs long distance 
	- high vs low speeds 

![[file-20250316203035013.png | 500]]

![[file-20250316203057472.png|500]]

![[file-20250316203152973.png|500]]

## Bluetooth 
- short range (10m - 100m)
	- controller-device 
	- 1-3Mbps
- frequencies
	- 2.4 GHz ~ 2.48 GHz
	- 79 channels, each 1MHz bandwidth 
- uses **Frequency Hopping Spread Spectrum** (FH-SS)
	- breaks data into packets, sends each one 1/79 channels
	- spread data *across many frequencies* avoids interference 
	- FH-SS performs 1600 hops per second 

![[file-20250316204142271.png | 500]]

- designed for cont. data transmission 
	- not good at bursts (common in sensor)
- BLE introduces sleep mode, wake up at transfer

![[file-20250316205233664.png | 500]]

