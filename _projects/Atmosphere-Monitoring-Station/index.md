---
layout: post
title: Atmoshpere Monitoring Station
description: Designed, manufactured, and programmed a custom 4-layer PCB for an atmospheric monitoring station capable of measuring temperature, humidity, and ambient light. The board integrates multiple sensors via I²C, providing accurate environmental data collection. 
skills: 
  - Altium Designer
  - C programming
  - STM32CubeIDE 
  - I2C protocol

main-image: /3Dpcb_title.png
---

## Schematic Creation and Component Selection 

{% include image-gallery.html images="Schem_mcu.png, Schem_modules.png, Schem_power.png" height="400" %} 


I carefully selected components to ensure compatibility with the microcontroller, communication protocols, and power supply requirements, while also confirming availability through my chosen manufacturer. Datasheets were thoroughly reviewed to verify pinouts, voltage ranges, and recommended application notes. 

The schematics were organized neatly into functional blocks (sensors, power regulation, SWD, MCU, and connectors) to improve readability. I also made sure to label all nets and ports with informative labels, this helped greatly for component placing and routing.

## Component Placement and Routing

{% include image-gallery.html images="Trace.png, GroundPlane.png" height="400" %} 


Components were strategically placed to minimize noise and optimize signal integrity. Regulator circuitry was located in the bottom-right section of the board to isolate noise from sensitive sensor components. Signal traces for the sensors were routed away from the external oscillator and voltage regulators to further reduce interference.

Power distribution was handled with wide polygon pours and thicker traces, while thinner traces were used for low-current signal lines. USB data lines were routed as 90-ohm differential pairs to maintain proper impedance. Vias and the bottom layer were used where necessary to relieve routing congestion, while the middle layers were dedicated copper pours for ground and 3.3 V. These planes provided strong return paths and significantly simplified routing.

## Board Features

{% include image-gallery.html images="3Dpcb.png" height="400" %} 


To improve usability and debugging, the board includes clear silkscreen labels for all major components, signal connections, and button functions. Care was taken to ensure the silkscreen is legible and does not overlap with any vias. Polarity indicators were added for screw terminals to prevent wiring mistakes, and labels were placed near power connectors and headers for quick identification.

LED indicators provide immediate feedback for USB power and battery power status, while a dedicated switch allows easy source selection. Physical placement of connectors was carefully considered during the design phase. Power inputs (USB and battery) were placed on the right side of the board, while GPIO pins and their associated 3.3 V and 5 V screw terminals were grouped together on the same edge to improve accessibility. The reset and BOOT0 buttons were intentionally positioned side by side, making it simple to run boot sequences when flashing firmware. These decisions enhance the user experience and make the board intuitive to use, especially during testing and troubleshooting.

## Programming 

{% include image-gallery.html images="SerialData.png" height="400" %} 


The firmware was developed in STM32CubeIDE using the HAL libraries to configure the microcontroller peripherals. The sensor datasheets were used to determine I²C protocol, specifically which adresses to configure and communicate with to receive data. This sensor communication was handled through HAL functions like: HAL_I2C_Mem_Write(), HAL_I2C_Master_Transmit(), and HAL_I2C_Master_Receive(). USB was configured as a virtual COM port allowing the board to stream sensor readings as formatted strings directly to a computer terminal. This provided a simple and effective way to verify sensor functionality and see data. 
