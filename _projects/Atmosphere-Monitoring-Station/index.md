---
layout: post
title: Atmoshpere Monitoring Station
description: Designed, manufactured, and programmed a custom 4-layer PCB for an atmospheric monitoring station capable of measuring temperature, humidity, and air pressure. The board integrates multiple sensors via I²C, providing accurate environmental data collection. 
skills: 
  - Altium Designer
  - C programming
  - STM32CubeIDE 
  - I2C protocol

main-image: /3Dpcb_title.png
---

## Schematic Creation and Component Selection 

{% include image-gallery.html images="Schem_mcu.png, Schem_modules.png, Schem_power.png" height="400" %} 

I took care to make sure components selected were compabitble and in stock with my chosen manufacturer. Thoroughly read data sheets for proper connections.

## Component Placement and Trace Layout

{% include image-gallery.html images="Trace.png" height="400" %} 

Power components are generally placed on the bottom right to avoid noise interference. Large poolygon pours used for power traces. Sensors data lines placed away from external oscillator and regulators. Trace widths taken into account, thinner for signals and thicker for power. Impedence controlled traces for USB lines (90ohmns). Used bottom layer for traces with vias when necessary 

## Silkscreen 

{% include image-gallery.html images="3Dpcb.png" height="400" %} 

Labelled everything for easy of use, +/- indicators, button functions, main components for debugging. Included LED indicators for USB power and battery power. 

## Programming 

Used STM32CubeIDE to configure device and program sensors using HAL library fufnctions for I2C. Setup USB as virtual COM port to send sensor readings as strings through USB and display on computer.

