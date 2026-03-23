---
layout: post
title: USB Current Monitor
description: Designed and manufactured a USB cable monitoring board that enables logging USB current and voltage using an external power analyzer. 
skills: 
  - Altium Designer
  - LPKF Protomat Circuit Board Plotter
  - Circuit Pro
  - Soldering (wires, SMD components)

main-image: /3Dpcb_title.png
---

## Design

{% include image-gallery.html images="Schem_mcu.png, Schem_modules.png, Schem_power.png" height="400" %} 

The current sensing capability is based on the Kelvin (4-wire) resistance measurement method. This method separates the measurement path from the current-carrying path, reducing voltage loss in the sensing circuit. Additionaly a low-pass RC filter is used to filter out any high frequency noise coming from the power supply. Large copper pours were used to further minimize voltage loss in the current path.  

Mechanical stability was considered by adding holes to secure the cable to the board with zip ties. Additional pads were included to measure USB voltage and to easily probe the true value of the resistor.



## Manufacturing

{% include image-gallery.html images="Trace.png, GroundPlane.png" height="400" %} 

**Highlights:**
- Independently manufactured the board using an LPKF Protomat circuit board plotter  
- Developed an understanding of the PCB manufacturing process and how Gerber files translate to the physical board, informing future design decisions  

## Resistor Calibration

**Process:**
- Measured the resistance of each resistor by applying a ramp current and plotting voltage versus current  
- Determined the resistance from the slope of the best-fit line to obtain an accurate average value  

## Final Product

{% include image-gallery.html images="3Dpcb.png" height="400" %} 

**Highlights:**
- Added wires to easily connect the board to a lab power analyzer, following best practices (used different lengths for power and ground wires to prevent accidental shorts)  
- Installed SMD components and wires using a soldering iron and heat gun  
- Added clear labels for usability and organization  
