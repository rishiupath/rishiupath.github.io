---
layout: post
title: Rover Motor Emergency Remote Stop
description: Designed a long-range emergency remote stop system for UBC Rover to meet competition safety requirements. Leading complete design, firmware, and CAD across 3 custom PCBs.
skills: 
  - Altium Designer
  - Relay
  - LoRa Radio
  - I2C, CAN FD, USB, UART
  - Power Management
main-image: /DRS_ALL.png
---

## Purpose
This project addresses one of the Canadian International Rover Competition's (CIRC) electrical safety requirements: rovers must have a remote motion stop capable of disabling a runaway rover, operating independently of the rover's primary communication system. 

The following project requirements are set: 
<ul style="color: #e0e0e0; line-height: 1.6; font-size: 16px;">
  <li>Must work at distances of 2–3 km</li>
  <li>Fully independent of the rover's existing communications</li>
  <li>Operate reliably under competition conditions</li>
</ul>

## System Overview
{% include image-gallery.html images="DRS_Block.png" height="400" %}
The system consists of 3 modules. The **transmitter module** is held by the operator, while the **receiver** and **switch modules** are mounted inside the rover. The operator uses the transmitter's buttons and OLED display to command a remote stop. The receiver picks up the LoRa signal and drives 6 GPIO outputs — one to each relay-based switch module — which sit inline between the battery and each motor driver.

---

## Receiver Module
{% include image-gallery.html images="Receiver_Block.png" height="400" %}
The receiver is powered from the rover's 24 V LiPo battery, stepped down to 3.3 V via a buck converter and LDO. A power mux prevents accidental back-feeding between USB and battery during development. Key components include an STM32 microcontroller, RN2903 LoRa module with antenna, USB for firmware flashing, CAN FD for optional integration with the rover's main bus, and 6 GPIO outputs to the switch modules.

### Schematics
{% include image-gallery.html images="" height="400" %}
### PCB Layout
{% include image-gallery.html images="" height="400" %}
### PCB 3D
{% include image-gallery.html images="" height="400" %}

---

## Switch Module
{% include image-gallery.html images="Switch_Block.png" height="400" %}
Each switch module sits between the power distribution board and a motor driver, interrupting the DC supply rail rather than the motor's 3-phase output — simplifying high-current switching. The core is an SPDT relay rated for high-current DC motor loads. A TVS diode suppresses inductive voltage spikes when the supply is cut, and a bleed resistor dissipates stored motor energy to slow coasting. The relay coil is powered directly from the 24 V input and switched via a BJT controlled by the wired signal from the receiver module.

### Schematics
{% include image-gallery.html images="" height="400" %}
### PCB Layout
{% include image-gallery.html images="" height="400" %}
### PCB 3D
{% include image-gallery.html images="" height="400" %}

---

## Transmitter Module
The transmitter is handheld and operator-carried. It accepts power from either 5× AA batteries or USB-C, managed through a power mux for seamless input switching. Reverse polarity protection is implemented with a P-channel MOSFET. Battery level is measured via ADC voltage sampling through a switched resistor divider (NMOS-gated) to minimize quiescent current, with ratiometric correction using the STM32's internal VREFINT reference. The module includes a LoRa radio and antenna, an OLED display showing system status, 6 individual motor-stop buttons, and 1 main stop button.

### Schematics
{% include image-gallery.html images="" height="400" %}
### PCB Layout
{% include image-gallery.html images="" height="400" %}
### PCB 3D
{% include image-gallery.html images="" height="400" %}

---

## Current Status
All three PCBs are in final review ahead of fabrication. Next steps are assembly, bring-up testing, and firmware development.
